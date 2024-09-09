postgre_fdw how to

# Connect to database												
psql -U on_sbp -d local_db
												
# install	fdw						
create extension postgres_fdw;												
												
# 外部サーバ定義の追加												
create server foreign_server foreign data wrapper postgres_fdw options (host 'localhost', port '5444', dbname 'remote_db');												
												
# ユーザマッピングの追加												
create user mapping for local_user server foreign_server options (user 'remote_user', password 'remote_password');												
												
# 権限の追加												
grant all on foreign server foreign_server to local_user;												
												
# 外部テーブルの追加												
import foreign schema "remote_user" limit to (tblname) from server foreign_server into local_user;												
												
# SQL												
select * from local_user.tblname;												
												
# 外部テーブルの削除												
drop foreign table local_user.tblname;												
												
# 権限のはく奪												
revoke all on foreign server foreign_server from local_user;												
												
# ユーザマッピングの削除												
drop user mapping for local_user server foreign_server;												
												
# 外部サーバ定義の削除												
drop server foreign_server;												
												
# FDWの削除												
drop extension postgres_fdw;												
