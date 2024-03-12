- [Husky 2024](#husky-2024)
- [References](#references)

### Husky 2024

Run CI on commit using Husky.

```
bun add -d husky
touch .husky/pre-commit
chmod +x .husky/pre-commit
```

`.husky/pre-commit`

```
bun ci
```

### References

- Homepage: https://typicode.github.io/husky/
- List of file hooks: https://git-scm.com/docs/githooks
- Gemini setup overview: https://g.co/gemini/share/40545d29fc61
