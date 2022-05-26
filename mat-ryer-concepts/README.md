# Tiny `main` abstraction

This abstraction helps when multiple things are going on in the `main` function. This allows you to return an error from the main setup, so that you don't have to repeat yourself multiple times; like when printing to stderr when an error occurs at one step in the setup. This way you only return that error and the print happens in `main`.

```go
func main() {
    if err := run(); err != nil {
        fmt.Fprintf(os.Stderr, "%s\n", err)
        os.Exit(1)
    }
}

func run() error {
    db, dbtidy, err := setupDatabase()
    if err != nil {
        return errors.Wrap(err, "setup database")
    }
    defer dbtidy()
    srv := &server{
        db: db,
    }
    ...
}
```

