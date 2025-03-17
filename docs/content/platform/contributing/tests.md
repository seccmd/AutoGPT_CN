# 测试

我们使用 [Playwright](https://playwright.dev/) 作为我们的测试框架。

## 开始之前

几乎所有的测试都需要你运行前端和后端服务器。如果你没有运行它们，你将会遇到奇怪且难以调试的错误，因为测试会尝试与应用程序交互，而应用程序并未处于可交互状态。

## 运行测试

要运行测试，你可以使用以下命令：

以无头模式运行测试，不显示 UI：

```bash
yarn test
```

如果你想在一个可以识别每个使用的定位器的 UI 中运行测试，你可以使用以下命令：

```bash
yarn test-ui
```

你还可以在测试命令中传递 `--debug` 参数，以在可见模式下打开浏览器，而不是无头模式。这适用于 `yarn test` 和 `yarn test-ui` 命令。

```bash
yarn test --debug
```

在 CI 中，我们以无头模式运行测试，使用多个浏览器，并在测试失败时重试最多 2 次。

你可以在 [playwright.config.ts](https://github.com/Significant-Gravitas/Autogpt/blob/master/autogpt_platform/frontend/playwright.config.ts) 中找到完整的配置。

### 调试测试

有很多不同的调试测试的方法。

我最喜欢的是结合使用 Playwright 的测试编辑器和 VSCode。

无论你做什么，你都应该**始终**仔细检查你的定位器是否正确。Playwright 经常会“超时”，并且不会给你定位器不正确的错误信息，因为它找不到元素。你可以通过浏览器中的开发者工具来检查，当你使用检查和选择元素工具时，它们应该在元素标签中可见。

#### 使用 Playwright 测试编辑器

如果你需要调试测试，你可以使用以下命令在 Playwright 测试编辑器中打开测试。如果你想在浏览器中查看测试，并查看测试时页面的状态和使用的定位器，这会很有帮助。

```bash
yarn test --debug --test-name-pattern="test-name"
```

#### 使用 VSCode

你可以安装 [Playwright Test for VSCode](https://marketplace.visualstudio.com/items?itemName=ms-playwright.playwright) 扩展，以获得 Playwright API 的自动补全（ID：`ms-playwright.playwright`）。

安装此扩展后，将在 VSCode 中启用 `Test Explorer` 视图，允许你运行、调试和查看当前项目中的所有测试。在你的测试中添加断点并运行它们将自动在正确的上下文中打开测试编辑器。

## 设置生成测试

使用 Playwright，你可以从现有的用户会话记录中生成测试。这对于创建更能代表用户如何与应用程序交互的测试非常有用。我们通常使用它来检查元素的 ID 以及需要添加 ID 的地方。

不断登录非常烦人，因此我强烈建议为你的测试使用保存的会话。
这将保存一个名为 `.auth/gentest-user.json` 的文件，可以在所有未来的生成测试中加载，这样你就不必每次都登录。

### 保存会话以供生成测试始终使用

```bash
yarn gentests --save-storage .auth/gentest-user.json
```

在你登录后，使用 `CTRL + C` 停止会话，并将 `--save-storage` 标志替换为 `--load-storage`，以加载会话供所有未来的测试使用。

### 加载会话以供生成测试始终使用

```bash
yarn gentests --load-storage .auth/gentest-user.json
```

## 如何创建新测试

测试由页面对象和测试文件组成。

页面对象是一个包含与页面交互的方法的类。

测试文件是一个包含页面或一组页面的测试的文件。

### 创建新的页面对象

对于测试，我们使用 [页面对象模型](https://playwright.dev/docs/pom)。这是一种模式，其中每个页面都是一个类，包含该页面的所有方法和定位器。
这对于保持测试的组织性和易读性非常有用，同时确保在 UI 更改时只需在一个地方更新测试。

你应该使用以下示例创建一个新的页面对象（仅在需要添加新页面或**跨多个测试的 UI 元素**时）。

我们扩展了 `BasePage` 类，该类包含具有共同功能的页面的共享方法，比如导航栏。如果你添加类似的东西（例如侧边栏），你应该将其添加到 `BasePage` 类中。否则，你应该创建一个新的页面对象。

每个页面对象应该在自己的文件中，并命名为 `page-name.page.ts`。
页面对象应该包含用户可以对该页面执行的操作的方法。例如，点击按钮、填写表单等。它还应该包含该页面独有的各种有用的抽象。例如，`BuildPage` 有一个连接块的方法。

这是一个简短的配置文件页面对象的示例：

```typescript title="frontend/src/tests/pages/profile.page.ts"
--8<-- "autogpt_platform/frontend/src/tests/pages/profile.page.ts:ProfilePageExample"
}
```

### 创建新的测试文件

对于测试，我们使用我们的页面对象来创建测试。每个测试文件应该在 `tests` 文件夹中，并命名为 `test-name.spec.ts`。一个测试文件可以包含多个测试。每个测试应该与相同的概念功能相关。例如，构建页面的测试文件可以有构建代理、创建输入和输出以及连接块的测试。如果你想专门测试构建代理，你可以创建一个名为 `building-agents.spec.ts` 的新测试。

测试可以从一个或多个页面对象继承，具有预操作和后操作，以及许多其他功能。你可以在这里了解更多关于不同功能以及如何使用它们的信息 [这里](https://playwright.dev/docs/test-actions)。

一个好的聚焦（`单元` 或 `单一概念`）测试将：

- 有一个简短的名称，描述它正在测试的内容
- 有一个单一的概念（构建代理、添加所有块、连接两个块等）
- 检查前置条件、操作和后置条件，并在过程中进行多次验证

一个好的非聚焦（`集成` 或 `多个概念`）测试将：

- 有一个简短的名称，描述它正在测试的内容
- 有多个概念（构建代理、创建->导出->导入->运行代理、以多种方式连接块，具有多个输入和输出等）
- 有一个明确的用户体验，确保其正常工作（例如，点击构建按钮并确保代理已构建，或点击导出按钮并确保代理已导出并显示在监控系统中）
- 不专注于单一概念，而是测试应用程序的整体流程。记住你不是在测试像素完美的 UI，而是用户体验。

一个好的测试套件将有一个健康的聚焦和非聚焦测试的组合。

### 示例聚焦测试及解释

```typescript title="frontend/src/tests/build.spec.ts"
--8<-- "autogpt_platform/frontend/src/tests/build.spec.ts:BuildPageExample"
});
```

1. `test.describe` 用于将测试分组。在这种情况下，它用于将所有构建页面的测试分组在一起。
2. `let buildPage: BuildPage;` 用于创建构建页面的新实例。
3. `test.beforeEach` 用于在每个测试之前运行代码。在这种情况下，它用于在每个测试之前登录用户。`page` 是从夹具传入的页面对象，`loginPage` 是登录页面的页面对象，`testUser` 是从夹具传入的用户对象。夹具用于处理身份验证和其他常见的共享状态任务。
4. `await page.goto("/login");` 用于导航到登录页面。
5. `await test.expect(page).toHaveURL("/");` 用于检查页面是否已导航到主页（因此已登录）。
6. `test("user can add a block", async ({ page }) => {` 用于定义一个新测试。
7. `await test.expect(buildPage.isLoaded()).resolves.toBeTruthy();` 用于检查构建页面是否已加载。这可以合理地放在 `test.beforeEach` 中，但为了清晰起见，在这里完成，因为此套件中有其他测试。
8. `await test.expect(page).toHaveURL(new RegExp("/.*build"));` 用于检查页面是否已导航到构建页面。
9. `await buildPage.closeTutorial();` 用于关闭构建页面上的教程，值得注意的是，这个包装函数实际上并不关心它是否打开，它确保它**将**被关闭。这是一个有用的常见模式，用于确保某事将被完成，而不关心它是否已经完成。它可以用于切换设置、关闭/打开侧边栏等。
10. `await buildPage.openBlocksPanel();` 用于打开构建页面上的块面板，与 `closeTutorial` 函数描述的方式相同。
11. `await buildPage.addBlock(block);` 用于将特定块添加到构建页面。这是另一个实用函数，可以在行内完成，但由于页面对象模式的工作方式，我们应该将它们保留在页面对象中。（它也有助于保持测试代码更清晰，并在其他测试中使用）
12. `await buildPage.closeBlocksPanel();` 用于关闭构建页面上的块面板。
13. `await test.expect(buildPage.hasBlock(block)).resolves.toBeTruthy();` 用于检查块是否已添加到构建页面。

### 在测试之间传递信息

你可以使用 `testInfo` 对象在测试之间传递信息。这对于在 `beforeAll` 之间传递代理 ID 非常有用，以便你可以为多个测试共享设置。

```typescript title="frontend/src/tests/monitor.spec.ts"
--8<-- "autogpt_platform/frontend/src/tests/monitor.spec.ts:AttachAgentId"

  test("test can read the agent id", async ({ page }, testInfo) => {
    --8<-- "autogpt_platform/frontend/src/tests/monitor.spec.ts:ReadAgentId"
    /// ... 在这里对代理 ID 做一些事情
  });
});
```

## 另请参阅

- [编写测试](https://playwright.dev/docs/writing-tests)
- [代码生成](https://playwright.dev/docs/codegen-intro)
- [测试 UI 模式](https://playwright.dev/docs/test-ui-mode)
- [跟踪查看器](https://playwright.dev/docs/trace-viewer-intro)
- [VSCode 入门](https://playwright.dev/docs/getting-started-vscode)
- [调试测试](https://playwright.dev/docs/debug)
- [测试夹具](https://playwright.dev/docs/test-fixtures)
- [全局设置和拆卸](https://playwright.dev/docs/test-global-setup-teardown)
- [测试参数化](https://playwright.dev/docs/test-parameterize)
- [测试事件](https://playwright.dev/docs/events)
- [测试组件](https://playwright.dev/docs/test-components)
- [测试分片](https://playwright.dev/docs/test-sharding)
- [无障碍测试](https://playwright.dev/docs/accessibility-testing)
- [身份验证](https://playwright.dev/docs/auth)
- [模拟](https://playwright.dev/docs/mock)
- [模拟浏览器 API](https://playwright.dev/docs/mock-browser-apis)
- [代码生成](https://playwright.dev/docs/codegen)
- [页面](https://playwright.dev/docs/pages)
- [测试注释](https://playwright.dev/docs/test-annotations)