- cookie区分路径、域名、名称但不区分端口号，session在客户端的体现依然是cookie
	- server.servlet.session.cookie.name是springboot同一域名下区分cookie的一种方式

- git pull = git fetch + git merge，区别是
	- 使用git fetch更新代码，本地的库中master的commitID不变
	- 使用git pull的会将本地的代码更新至远程仓库里面最新的代码版本