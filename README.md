# py-kms Aster
![Description Image](https://hl-cdn.horace.seesea.site/wp-content/uploads/2024/07/WX20240705-123017@2x-1024x511.png)

# English Version
This project is modified from **py-kms**, it is a python-based KMS server for Windows and Office products.

The data is stored using sqlite3, with a file path of Docker internal/home/py kms/db/. This path can be mounted locally for easy data retention during subsequent upgrades.

### Port

This project uses two ports:
- KMS port: 1688
- WebUI port: 8080

It can be modified according to the actual situation, such as mapping 1688 to 12345. After modifying to another port, it is necessary to add the port after the server address of KMS on Windows.

<code>docker run -d —p 1688:1688 -p 8080:8080 -v `<Change it>`:/home/py-kms/db asterinstitute/py-kms:1.1</code>
  
### Blacklist Function

Edit `pykms_database.db`:
```
sqlite3 pykms_database.db
```
Database structure:
```
sqlite> .table
black_list  clients   
sqlite> .schema
CREATE TABLE clients(clientMachineId TEXT , machineName TEXT, applicationId TEXT, skuId TEXT, licenseStatus TEXT, lastRequestTime INTEGER, kmsEpid TEXT, requestCount INTEGER, clientIP TEXT, PRIMARY KEY(clientMachineId, applicationId));
CREATE TABLE black_list(clientMachineId TEXT, machineName TEXT, addTime INTEGER);
sqlite>
```
clients table are saving the client which use this KMS server to activate，black_list are blacklist.

By insert record into table black_list, add blocked client.
```
INSERT INTO black_list VALUES (machineId", "machineName", "Unix Timestamp (Optional)");
```
MachineId and MachineName can only be written as one entry.

# 中文文档
此项目为**py-kms**的二次开发，这是一个基于 Python 的微软 Windows 与 Office 激活服务器（KMS）。

数据采用 sqlite3 进行存储，文件路径为 Docker 内部`/home/py-kms/db/pykms_database.db`，将此路径挂载在本地，方便后续升级数据留存。

### 端口

此项目用到两个端口：
- KMS 端口：1688
- WebUI 端口：8080

可根据实际情况修改，比如将 1688 映射到 12345 。修改到其他端口后需要在 Windows 上设置 kms 的服务器地址后面加上端口。

<code>docker run -d —p 1688:1688 -p 8080:8080 -v <修改为本地路径>:/home/py-kms/db asterinstitute/py-kms:1.1</code>
  
### 黑名单功能
为了方便管理连接到 kms 服务器的客户端，本项目增加了黑名单功能，目前需要用户手动到数据库文件修改。

编辑`pykms_database.db`文件：
```
sqlite3 pykms_database.db
```
此数据库文件的结构如下：
```
sqlite> .table
black_list  clients   
sqlite> .schema
CREATE TABLE clients(clientMachineId TEXT , machineName TEXT, applicationId TEXT, skuId TEXT, licenseStatus TEXT, lastRequestTime INTEGER, kmsEpid TEXT, requestCount INTEGER, clientIP TEXT, PRIMARY KEY(clientMachineId, applicationId));
CREATE TABLE black_list(clientMachineId TEXT, machineName TEXT, addTime INTEGER);
sqlite>
```
clients存储的是所有使用此KMS激活的客户端，black_list存储着黑名单。

可以通过SQL语句新增黑名单
```
INSERT INTO black_list VALUES ("替换为machineId", "替换为machineName", "Unix时间戳（可选）");
```
machineId和machineName只写入一项即可。

# 更新日志 / Update log
V1.1 - 2024-07-05: Add blacklist function.<br>
V1.0 - 2024-07-03: Add client IP Address to WebUI.

# Modified py-kms
Originally from Github: https://github.com/SystemRage/py-kms

