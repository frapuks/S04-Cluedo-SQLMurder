# SQL MURDER

## Ressource

[SQL Murder](https://mystery.knightlab.com/)

## 1. Check police report

```sql
SELECT *
FROM crime_scene_report
where date = 20180115
AND city = 'SQL City'
AND type = 'murder'
```

Résultat :
| date     | type   | description                                                                                                                                                                               | city     |
| -------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| 20180115 | murder | Security footage shows that there were 2 witnesses. The first witness lives at the last house on "Northwestern Dr". The second witness, named Annabel, lives somewhere on "Franklin Ave". | SQL City |

## 2. Get witness interview

```sql
SELECT name,transcript
FROM interview
JOIN person ON person_id = person.id
WHERE (name LIKE '%Annabel%' AND address_street_name = 'Franklin Ave')
OR (address_street_name = 'Northwestern Dr')
ORDER BY address_street_name,address_number DESC
LIMIT 2
```

Résultat :
| name           | transcript                                                                                                                                                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Annabel Miller | I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th.                                                                                                           |
| Morty Schapiro | I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W". |

## 3. Find murderer

```sql
SELECT get_fit_now_member.id, person.name, membership_status, plate_number, check_in_date, check_in_time, check_out_time 
FROM get_fit_now_check_in
JOIN get_fit_now_member ON get_fit_now_member.id = membership_id
JOIN person ON person.id = person_id
JOIN drivers_license ON drivers_license.id = license_id
WHERE check_in_date = 20180109
AND membership_status = 'gold'
AND get_fit_now_member.id LIKE '48Z%'
AND plate_number LIKE '%H42W%'
AND (check_in_time BETWEEN (SELECT check_in_time FROM get_fit_now_check_in WHERE membership_id=90081) AND (SELECT check_out_time FROM get_fit_now_check_in WHERE membership_id=90081)
OR check_out_time BETWEEN (SELECT check_in_time FROM get_fit_now_check_in WHERE membership_id=90081) AND (SELECT check_out_time FROM get_fit_now_check_in WHERE membership_id=90081))
```

Résultat :
| id    | name           | membership_status | plate_number | check_in_date | check_in_time | check_out_time |
| ----- | -------------- | ----------------- | ------------ | ------------- | ------------- | -------------- |
| 48Z55 | Jeremy Bowers  | gold              | 0H42W2       | 20180109      | 1530          | 1700           |

## 4. Get murderer interview

```sql
SELECT name,transcript
FROM interview
JOIN person ON person_id = person.id
WHERE name = 'Jeremy Bowers'
```

Résultat :
| name          | transcript                                                                                                                                                                                                                                       |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Jeremy Bowers | I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017. |

## 5. Find the culpable

```sql
SELECT name, gender, hair_color, height, car_make, car_model, event_name, facebook_event_checkin.date
FROM person
JOIN facebook_event_checkin ON facebook_event_checkin.person_id = person.id
JOIN drivers_license ON license_id = drivers_license.id
WHERE event_name = 'SQL Symphony Concert'
AND facebook_event_checkin.date BETWEEN 20171201 AND 20171231
AND gender = 'female'
AND hair_color = 'red'
AND car_make = 'Tesla'
AND car_model = 'Model S'
AND height BETWEEN 65 AND 67
```

Résultat :
| name             | gender | hair_color | height | car_make | car_model | event_name           | date     |
| ---------------- | ------ | ---------- | ------ | -------- | --------- | -------------------- | -------- |
| Miranda Priestly | female | red        | 66     | Tesla    | Model S   | SQL Symphony Concert | 20171206 |
| Miranda Priestly | female | red        | 66     | Tesla    | Model S   | SQL Symphony Concert | 20171212 |
| Miranda Priestly | female | red        | 66     | Tesla    | Model S   | SQL Symphony Concert | 20171229 |

## 6. Ask to the judge

```sql
INSERT INTO solution VALUES (1, 'Miranda Priestly');
SELECT value FROM solution;
```

Résultat :
| value    |     |
| -------- | --- |
| Congrats |     you found the brains behind the murder! Everyone in SQL City hails you as the greatest SQL detective of all time. Time to break out the champagne! |
