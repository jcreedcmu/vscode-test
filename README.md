<p>
  <h1 align="center">vscode-test</h1>
</p>

<p align="center">
  <a href="https://dev.azure.com/vscode/vscode-test/_build?definitionId=15">
    <img src="https://img.shields.io/azure-devops/build/vscode/350ef5c4-15fc-411a-9a5e-0622da4da69c/15.svg?label=Azure%20DevOps&logo=Azure%20Devops&style=flat-square">
  </a>
  <a href="https://travis-ci.org/microsoft/vscode-test">
    <img src="https://img.shields.io/travis/microsoft/vscode-test.svg?label=Travis&logo=Travis&style=flat-square">
  </a>
</p>

This module helps you test VS Code extensions.

Supported:

- Node >= 8.x
- Windows >= Windows Server 2012+ / Win10+ (anything with Powershell >= 5.0)
- macOS
- Linux

## Usage

See [./sample](./sample) for a runnable sample, with [Azure Devops Pipelines](https://github.com/microsoft/vscode-test/blob/master/azure-pipelines.yml) configuration.

```ts
import * as path from 'path'

import { runTests, downloadAndUnzipVSCode } from 'vscode-test'

async function go() {

  const extensionDevelopmentPath = path.resolve(__dirname, '../../')
  const extensionTestsPath = path.resolve(__dirname, './suite')
  const testWorkspace = path.resolve(__dirname, '../../test-fixtures/fixture1')

  /**
   * Basic usage
   */
  await runTests({
    extensionDevelopmentPath,
    extensionTestsPath,
    launchArgs: [testWorkspace1]
  })

  const extensionTestsPath2 = path.resolve(__dirname, './suite2')
  const testWorkspace2 = path.resolve(__dirname, '../../test-fixtures/fixture2')

  /**
   * Running a second test suite
   */
  await runTests({
    extensionDevelopmentPath,
    extensionTestsPath: extensionTestsPath2,
    launchArgs: [testWorkspace2]
  })

  /**
   * Use 1.31.0 release for testing
   */
  await runTests({
    version: '1.31.0',
    extensionDevelopmentPath,
    extensionTestsPath,
    launchArgs: [testWorkspace]
  })

  /**
   * Use Insiders release for testing
   */
  await runTests({
    version: 'insiders',
    extensionDevelopmentPath,
    extensionTestsPath,
    launchArgs: [testWorkspace]
  })

  /**
   * Manually download VS Code 1.31.0 release for testing.
   * Noop, since 1.31.0 already downloaded by the previous command to '.vscode-test/vscode-1.31.0'.
   */
  await downloadAndUnzipVSCode('1.31.0')

  /**
   * Manually download VS Code 1.30.0 release and run tests on it.
   */
  const vscodeExecutablePath = await downloadAndUnzipVSCode('1.30.0')
  await runTests({
    vscodeExecutablePath,
    extensionDevelopmentPath,
    extensionTestsPath,
    launchArgs: [testWorkspace]
  })

  /**
   * - Add additional launch flags for VS Code.
   * - Pass custom environment variables to test runner.
   */
  await runTests({
    vscodeExecutablePath,
    extensionDevelopmentPath,
    extensionTestsPath,
    launchArgs: [
      testWorkspace,
      // This disables all extensions except the one being testing.
      '--disable-extensions'
    ],
    // Custom environment variables for extension test script.
    extensionTestsEnv: { foo: 'bar' }
  })
}

go()
```

## License

[MIT](LICENSE)

## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
