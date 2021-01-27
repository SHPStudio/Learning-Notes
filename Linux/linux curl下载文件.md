# curl下载github私有仓库文件
`curl -H 'Authorization: token INSERTACCESSTOKENHERE' -H 'Accept: application/vnd.github.v3.raw' -O -L https://api.github.com/repos/owner/repo/contents/path`