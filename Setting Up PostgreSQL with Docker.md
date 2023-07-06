```bash
docker run --name postgresql -e POSTGRES_PASSWORD=yourpassword -e POSTGRES_USER=your_user -e POSTGRES_DB=your_db -p 5432:5432 -d postgres:13.4-alpine
```

`-- name` set container's name
`-e`  set enviroment variable
`-p` port's mapping
`-d` run container detached (in background)

http://localhost:5432

[[Database]]
[[PostgreSQL]]
[[Docker]]
