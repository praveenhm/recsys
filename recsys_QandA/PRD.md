I’ll lock scope and requirements with targeted questions. Once clarified, I’ll propose the end-to-end architecture and phase plan.

### Objectives and success criteria
- What is the primary optimization target? Choose one: 
  - GMV (price × conversion probability), net marketplace revenue (commission/fees), or gross profit (margin after shipping/promo costs).
- How should we balance conversion vs revenue? e.g., weighted objective with tunable alpha.
- Recommendation surfaces in scope for this batch system (home feed, category feed, search empty-state, notifications/email)? Any surface-specific constraints or K values (e.g., top-100 per user)?
- Geography/timezone scope (Japan-only? multi-region)? Language handling (JP/EN multilang)?

### Data availability and schemas (BigQuery)
- Project/dataset names for all source tables; confirm table names for events:
  - Purchases, views, searches, favorites/likes, cart/add-to-watchlist, and returns/cancellations.
- For each event table, provide: column names for `user_id`, `item_id`, timestamps, event metadata, dedupe keys, and partitions/clustering.
- Retention windows: how far back do events go; desired training window (e.g., 180/365 days)?
- Timezone standardization (UTC vs JST) and late-arriving event SLA.
- Catalog tables:
  - `items`: fields for price, currency, shipping cost, category_id(s), condition, brand, title/description text, image URLs/features, seller_id, item status (available/sold), created_at, sold_at, quality flags.
  - `item_category`: taxonomy depth, path/hierarchy fields, multilingual labels.
- User tables:
  - `users`: inferred purchase power (e.g., spend decile), location, device, cohorts, account status; any PII constraints (e.g., only hashed IDs)?
- Inventory freshness: how fast do items sell after listing; is there a reliable “available” flag updated near-real-time?

### Revenue/cost modeling
- Commission model: category-level fee schedule available? Are shipping fees borne by buyer/seller and do we have cost fields?
- Promotions/discounts and coupons: can we access realized net revenue per transaction?
- Return/refund/cancellation rates by category/seller available for risk-adjusted revenue?
- Do we need to enforce price fairness vs user purchase power (e.g., avoid recommending items priced far above user’s typical spend)?

### Business rules and constraints
- Must-exclude items: adult categories, recalled, prohibited brands, fakes, unsafe sellers, own listings (user_id == seller_id), out-of-stock/sold, stale listings.
- Diversity requirements: category/brand diversity, exploration slots, seller fairness, long-tail exposure.
- Seller reliability signals: cancellation rate, seller rating, SLA breaches—available and required as constraints or features?

### Signals and features
- Confirm signal priority and availability for:
  - Purchases (strong), views and dwell/time-on-detail (medium), searches and query terms (intent), favorites/likes/watchlist (high), add-to-cart (very high), messages/inquiries (if available).
- Content features available now/later:
  - Text (title/description in JP/EN), images (raw or precomputed embeddings), taxonomy hierarchies.
- Temporal features:
  - Trending/popularity decay windows (7/14/28 days), seasonality (month/week), event-time leakage handling.

### Modeling approach preferences
- Two-stage architecture confirmation:
  - Candidate generation: item-item co-occurrence (purchase and view→purchase), popularity/trending, category-personalized, possibly MF embeddings.
  - Ranking: revenue-aware model combining conversion probability with price/margin/inventory/seller reliability.
- Tech stack constraints:
  - BigQuery ML only vs allowance for Vertex AI models trained in Colab and scored in BigQuery.
  - Are deep models (Neural CF/Two-Tower) in scope now or later?
- Cold-start strategy:
  - New users: contextual priors (geo, device, referrer), trending/category popular.
  - New items: content similarity via text/image/taxonomy; do we have embeddings now?

### Evaluation and experimentation
- Offline metrics to report: RPU (expected revenue uplift), conversion rate, CTR, coverage/diversity, calibration. Any business-preferred baselines?
- Temporal split: minimum holdout horizon (e.g., last 4 weeks) and blackout window to avoid leakage.
- A/B testing framework:
  - Existing experiment platform? Unit of randomization (user), target sample size and duration, guardrails (complaints, return rates).
  - Surfaces to test first; exposure caps per user/day.

### Batch frequency, SLAs, and storage
- Update cadence:
  - Models weekly vs features daily; scoring daily was requested—confirm this frequency and SLAs (e.g., ready by 06:00 JST).
- Output contract:
  - Destination table spec for recommendations (e.g., `user_id`, `item_id`, rank, score, reason_code, expiry_at, feature snapshots).
  - Top-N per user size and TTL.
- Realtime safeguards:
  - Should we perform a fast post-filter at request time to drop sold items (even in a batch system), or accept occasional staleness?

### Scale, cost, and ops
- Data scale rough order of magnitude: daily events (views, purchases), catalog size, active users.
- BigQuery slots/budget constraints (on-demand vs reservations). Any quota limitations we must respect?
- Scheduling/orchestration preferences: Cloud Composer vs Cloud Scheduler + BigQuery scheduled queries; artifact/versioning expectations.
- Data quality checks required (DQ rules, anomaly alerts), and monitoring for drift/perf regression.
- Access and privacy: PII policy, cross-device identity resolution, GDPR/CCPA constraints for training and serving.

### Guardrails and explainability
- Required “reason codes” for recommendations (e.g., “Because you viewed X”, “Popular in your category”). Which types are acceptable?
- Business override rules: curated boosts, blacklists/whitelists per campaign/category.

If you confirm/answer the above (unknowns can be “use default”), I’ll produce:
- The exact end-to-end design (two-stage architecture, tables, dependencies).
- The concrete feature sets and aggregation specs.
- The revenue objective formula and ranking score composition.
- The batch schedule/SLAs and the production rollout plan on GCP.