# Profiler Backend

The backend for the Profiler App

## Attributions

- The [Golang](https://go.dev/) Team for creating my favorite language
- [SQLBoiler](https://github.com/volatiletech/sqlboiler) for a fantastic DB-First ORM
- [GORM](https://gorm.io/) 

## Roadmap

- Migration to [gqlgen](https://gqlgen.com/) or [Bob](https://github.com/stephenafamo/bob)

## What you'll need

- [Go](https://go.dev/)
- [sqlboiler](https://github.com/volatiletech/sqlboiler)
- a Database (We developed against postgres)

## Things I recommend

- [air](https://github.com/air-verse/air)
- [WSL](https://learn.microsoft.com/en-us/windows/wsl/install/Linux) (Because I built a few helper-scripts to get you started)
- [Podman for Windows](https://github.com/containers/podman/blob/main/docs/tutorials/podman-for-windows.md) / [Podman for Linux](https://podman.io/docs/installation#installing-on-linux)

## Important Env-Vars

| Var                   | Default                   | Usage     | 
|---                    |---                        |---        |
| DB_HOST               | localhost                 |           |  
| DB_PORT               | 5432                      |           |  
| DB_DRIVER             | postgres                  |           |  
| DB_USERNAME           | postgres                  |           |  
| DB_PASSWORD           | myawesomepassword         |           |  
| DB_NAME               | profiler                  |           |  
| MODE                  | development               | Legacy Remnant: Deactivates all Token-Checks  |  
| RENDER_SERVICE_URL    | https://localhost:3001    | URL to the renderservice, only necessary for generating DOCX-Documents  |  

## How to run it

### Filling the DB

In [testdata/crud/](testdata/crud/) and [testdata/prefill/profiler](testdata/prefill/profiler/) you'll find scripts to create and fill the DB in order with necessary data.
The script [testdata/prefill/profiler/99-Create-User.sql](testdata/prefill/profiler/99-Create-User.sql) will generate a demo User. If you didn't change the Data of the demo-user don't worry about it, you'll be able to change everything (that is not the primary key) in the app after the fact. 


#### Helper Script

Here is a little helper I use to set up the DB in a single command: 

```bash
setupDB() {
    psql -f testdata/crud/0-DropDB.sql
    psql -f testdata/crud/1-CreateDB.sql
    for f in testdata/prefill/profiler/*.sql;
    do
        psql -f "$f"
    done 
    for f in testdata/prefill/profiler/skills/*.sql;
    do
        psql -f "$f"
    done 
}
```

### Running / Debugging

Either you use `air`, `go run main.go`, or if you want to **debug**, you can use the following VS-Code Launch Config:

```json
{
    "name": "Debug: Backend",
    "type": "go",
    "request": "launch",
    "mode": "debug",
    "program": "backend/main.go",
    "env": {
        "DB_HOST": "localhost",
        "DB_USERNAME": "postgres",
        "DB_PASSWORD": "myawesomepassword",
        "RENDER_SERVICE_URL": "http://localhost:3001/",
        "DB_NAME": "profiler",
        "DB_PORT": "5432",
        "MODE": "development"
    }
}
```
