# test

DELETE FROM your_table t
WHERE t.date2 < (
  SELECT MAX(t2.date2)
  FROM your_table t2
  WHERE t2.date1 = t.date1
);


DELETE FROM your_table
WHERE ROWID IN (
  SELECT rid
  FROM (
    SELECT ROWID rid,
           ROW_NUMBER() OVER (
             PARTITION BY date1
             ORDER BY date2 DESC, id DESC
           ) rn
    FROM your_table
  )
  WHERE rn > 1
);


DELETE FROM your_table t
WHERE (t.date1, t.date2, t.id) NOT IN (
  SELECT date1,
         MAX(date2),
         MAX(id) KEEP (DENSE_RANK LAST ORDER BY date2)
  FROM your_table
  GROUP BY date1
);

///

SELECT *
FROM your_table
WHERE ROWID IN (
  SELECT rid
  FROM (
    SELECT ROWID rid,
           ROW_NUMBER() OVER (
             PARTITION BY date1
             ORDER BY date2 DESC, id DESC
           ) rn
    FROM your_table
  )
  WHERE rn > 1
);

DELETE FROM your_table
WHERE ROWID IN (
  SELECT rid
  FROM (
    SELECT ROWID rid,
           ROW_NUMBER() OVER (
             PARTITION BY date1
             ORDER BY date2 DESC, id DESC
           ) rn
    FROM your_table
  )
  WHERE rn > 1
);
