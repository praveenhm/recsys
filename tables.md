
### user_events table joined with item_category table -> events_enriched_final
SELECT * 
FROM `auxia-reporting.temp_dataset_praveen_tokyo.events_enriched_final`

user_id	event_name	item_id	category_id	category_full_name_en	price	seller_id	client_event_timestamp
168193466	ITEM_LIKE	m41859481159	119	Fashion > Women > Tops > T -Shirt / Cut -And -Sew > Short Sleeve (T -Shirt)	2800	127667696	2025-08-22 05:50:03.334156 UTC
430118094	ITEM_LIKE	m40597136207	119	Fashion > Women > Tops > T -Shirt / Cut -And -Sew > Short Sleeve (T -Shirt)	7800	718305088	2025-08-27 08:31:05.033415 UTC
475420478	ITEM_LIKE	m22381622424	119	Fashion > Women > Tops > T -Shirt / Cut -And -Sew > Short Sleeve (T -Shirt)	3600	163224569	2025-08-22 13:07:55.384397 UTC
754967445	ITEM_LIKE	m30511322746	119	Fashion > Women > Tops > T -Shirt / Cut -And -Sew > Short Sleeve (T -Shirt)	3399	317466276	2025-08-23 01:03:30.915084 UTC
124129585	ITEM_LIKE	m69953250317	119	Fashion > Women > Tops > T -Shirt / Cut -And -Sew > Short Sleeve (T -Shirt)	400	124129585	2025-08-25 10:57:37.102082 UTC
756167350	ITEM_LIKE	m20166492768	119	Fashion > Women > Tops > T -Shirt / Cut -And -Sew > Short Sleeve (T -Shirt)	2800	127115786	2025-08-25 10:23:16.400202 UTC
710202377	ITEM_LIKE	m67904323673	119	Fashion > Women > Tops > T -Shirt / Cut -And -Sew > Short Sleeve (T -Shirt)	9800	667728629	2025-08-24 15:08:18.800005 UTC
657486699	ITEM_LIKE	m88071262993	119	Fashion > Women > Tops > T -Shirt / Cut -And -Sew > Short Sleeve (T -Shirt)	1780	973513005	2025-08-24 09:53:56.123908 UTC
221249469	ITEM_LIKE	m15445775360	119	Fashion > Women > Tops > T -Shirt / Cut -And -Sew > Short Sleeve (T -Shirt)	4500	314221459	2025-08-25 22:34:30.485323 UTC
416469126	ITEM_LIKE	m26426436401	119	Fashion > Women > Tops > T -Shirt / Cut -And -Sew > Short Sleeve (T -Shirt)	2990	603242542	2025-08-25 11:06:20.396426 UTC

## Event_name can be one of them
1	ITEM_VIEW
2	ITEM_LIKE
3	PURCHASE_START
4	PURCHASE_COMPLETED


SELECT
  *
FROM
  `auxia-reporting.temp_dataset_praveen_tokyo.item_features_final`;

item_id	seller_id	status	price	category_id	category_full_name_en	updated_timestamp
m49352012397	498469624	on_sale	580	1	Fashion > Women	2025-08-22 10:02:00.000000 UTC
m35668767929	773289413	on_sale	300	7	"Smartphone, Tablet, Personal Computer"	2025-08-21 09:39:06.000000 UTC
m37069907783	515425551	on_sale	780	11	Fashion > Women > Tops	2025-08-19 08:02:43.000000 UTC
m41731478308	618574822	on_sale	680	11	Fashion > Women > Tops	2025-08-17 12:22:06.000000 UTC
m42760039733	429438386	on_sale	350	13	Fashion > Women > Pants	2025-08-26 00:58:57.000000 UTC
m45624570086	618574822	on_sale	480	14	Fashion > Women > Skirt	2025-08-17 11:56:38.000000 UTC
m81678907816	472813407	on_sale	2380	15	Fashion > Women > One Piece	2025-08-15 04:46:22.000000 UTC
m10319791966	532586097	on_sale	4100	16	Fashion > Women > Shoes	2025-08-26 14:22:18.000000 UTC
m77955148813	311795126	on_sale	8500	16	Fashion > Women > Shoes	2025-08-25 02:12:39.000000 UTC
m59554543512	916357810	on_sale	14700	16	Fashion > Women > Shoes	2025-08-26 12:30:28.000000 UTC



SELECT
  *
FROM
  `auxia-reporting.temp_dataset_praveen_tokyo.simple_user_events`;

user_id	event_name	item_id	category_level1_name_en	category_level2_name_en	client_event_timestamp
396708814	ITEM_LIKE	m18953324731	Baby Kids	Baby Kids.Baby Clothes (~ 95Cm)	2025-08-27 21:31:32.717924 UTC
961654949	ITEM_LIKE	m12352387364	Baby Kids	Baby Kids.Baby Clothes (~ 95Cm)	2025-08-27 16:12:50.127947 UTC
462209691	ITEM_LIKE	m66076973344	Baby Kids	Baby Kids.Baby Clothes (~ 95Cm)	2025-08-27 22:25:55.911486 UTC
447989071	ITEM_LIKE	m51333931059	Baby Kids	Baby Kids.Baby Clothes (~ 95Cm)	2025-08-27 16:10:02.808370 UTC
702574578	ITEM_LIKE	m36415789264	Baby Kids	Baby Kids.Baby Clothes (~ 95Cm)	2025-08-27 11:23:59.792589 UTC
925796716	ITEM_LIKE	m38051216809	Baby Kids	Baby Kids.Baby Clothes (~ 95Cm)	2025-08-27 13:50:49.050046 UTC
220969931	ITEM_LIKE	m32204311107	Baby Kids	Baby Kids.Baby Clothes (~ 95Cm)	2025-08-27 00:15:47.988262 UTC
429393030	ITEM_LIKE	m44450075945	Baby Kids	Baby Kids.Baby Clothes (~ 95Cm)	2025-08-27 04:36:02.495922 UTC
755772949	ITEM_LIKE	m84681915836	Baby Kids	Baby Kids.Baby Clothes (~ 95Cm)	2025-08-27 10:23:22.472157 UTC
990772635	ITEM_LIKE	m32575015461	Baby Kids	Baby Kids.Baby Clothes (~ 95Cm)	2025-08-27 10:22:22.819128 UTC  