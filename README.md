# SQL-Injection-Detection-Engineering-DVWA-
SQL Injection on DVWA

**Step for SQL Injection on Damn Vulnerable Web Application**
1. Accessing DVWA web page on home browser: *http://<ubuntu server IP>:8081*
2. Login using admin credential username and password
3. Go to the left tab > SQL Injection
4. Input "1' OR '1'='1" on User ID section

   <img width="901" height="871" alt="1" src="https://github.com/user-attachments/assets/28c3e084-cd13-4a4e-a8d6-943e2ba23b99" />

5. After that, immediately go to Parrot terminal and initiate the *sqlmap* command:

```bash
sqlmap -u "http://192.168.1.15:8081/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie="PHPSESSID=q3ubpfr4r4nt4osa1kv3chtj55; security=low" --dbs
```

<img width="1000" height="775" alt="2" src="https://github.com/user-attachments/assets/0d306f63-c6af-41ee-b8a4-46fa1add2b89" />


6. At the same time, visit the DVWA web page and copy the `PHPSESSID` 

<img width="2021" height="976" alt="3" src="https://github.com/user-attachments/assets/6edd898b-10e9-44b9-b376-128273f15e37" />


7. Go to 'Wazuh-Manager' terminal and edit the following rules for *wazuh detection rule*, and paste the following command:

```bash
<group name="web,attack,sqli,">
  <rule id="100001" level="10">
    <if_sid>31100</if_sid>
    <match>UNION|SELECT|information_schema|ORDER BY|'--</match>
    <description>Inbound Web Application Attack: SQL Injection Signatures Detected in URI String</description>
    <mitre><id>T1190</id></mitre>
  </rule>
</group>
```

<img width="993" height="483" alt="4" src="https://github.com/user-attachments/assets/208926c8-f94e-42b4-9dde-bf832da7ad93" />

8. Once done, press `ctl+O` and after that `ctl+X` and save.
9. **Restart** the Wazuh-Manager:
    ```bash
    sudo systemctl restart wazuh-manager
    ```
