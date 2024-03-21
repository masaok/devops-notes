- [Sharing a module](#sharing-a-module)
  - [Prepare `sharedmodule`](#prepare-sharedmodule)
  - [Link to it from project](#link-to-it-from-project)
  - [Develop and Test](#develop-and-test)
  - [Unlink when done](#unlink-when-done)

### Sharing a module

#### Prepare `sharedmodule`

```
cd /path/to/sharedmodule
yarn link
```

Note: Make sure package.json includes a `name` field, e.g., `name`: `sharedmodule`.

#### Link to it from project

```
cd /path/to/NextJSApp
yarn link sharedmodule
```

#### Develop and Test

Make your changes in the /path/to/SharedTSModule directory. Because the other projects are linked to this local version, changes will be reflected in NextJSApp and NodeExpressAPI when they're rebuilt or restarted.
Test your changes within these projects as needed.

#### Unlink when done

Step 4: Unlink When Done
After testing, or when you're ready to switch back to a published version of SharedTSModule, you'll want to unlink:

```
yarn unlink sharedmodule
```

Then, to ensure each project uses the published package from the registry, add it back with `yarn add sharedtsmodule` if you've published the updated module.

You generally don't need to unlink the package globally unless you want to clean up. If desired, run yarn unlink in the /path/to/SharedTSModule directory.

https://chat.openai.com/share/8a6470a7-2e2a-48a2-a304-68e0f65ef83b
