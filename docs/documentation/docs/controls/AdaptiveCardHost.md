# Adaptive Card Host

The motivation behind this control is to have a React control that facilitates the use of Adaptive Cards in SPFx by adding some features such as:

- Graphic integration with SharePoint / Microsoft Teams themes, both as regards the color palette and the use of the Fluent UI React controls instead of the classic HTML controls.

- Automatic use of Adaptive Cards Templating if an object containing data is passed to the control.

For graphic integration, in the case of SharePoint the current theme (page or section theme) or a custom theme is used.
Three custom themes have been created for Microsoft Teams to emulate the colors of the available themes: Default, Dark and Hight Contrast.

All Elements and Actions of Adaptive Cards have been redefined using Fluent UI React, both for SharePoint and Microsoft Teams (in this case the "Fluent UI Northstar" library is not used), adding and improving features that are not managed in Microsoft's implementation of the "adaptivecards-fluentui" library (Theme support for example).

The Adaptive Cards version supported is 1.5, by using the 'adaptivecards' npm package version 2.10.0.

Here is an example of the control in action inside a Web Part:

![Adaptive Card Host control](../assets/AdaptiveCardHost.gif)

Here is an example of the previous Web Part (using different Card), hosted as a Tab in Microsoft Teams:

![Adaptive Card Host control](../assets/AdaptiveCardHostTeams.gif)

## How to use this control in your solutions

- Check that you installed the `@pnp/spfx-controls-react` dependency. Check out the [getting started](../../#getting-started) page for more information about installing the dependency.
- In your component file, import the `AdaptiveCardHost` control as follows:

```TypeScript
import { AdaptiveCardHost, IAdaptiveCardHostActionResult, AdaptiveCardHostThemeType } from "@pnp/spfx-controls-react/lib/AdaptiveCardHost";
```

- Example on use the `AdaptiveCardHost` control with only required properties:

```TSX
<AdaptiveCardHost
  card={card}
  onInvokeAction={(action) => alert(JSON.stringify(action))}
  onError={(error) => alert(error.message)}
/>
```

- Example on use the `AdaptiveCardHost` control with all properties:

```TSX
<AdaptiveCardHost
  card={card}
  data={data}
  style={null}
  className={null}
  theme={this.props.theme}
  themeType={themeType}
  hostConfig={null}
  onInvokeAction={(action) => alert(JSON.stringify(action))}
  onError={(error) => alert(error.message)}
  onSetCustomElements={(registry: CardObjectRegistry<CardElement>) => {
    registry.register("CustomElementName", CustomElement);
  }}
  onSetCustomActions={(registry: CardObjectRegistry<Action>) => {
    registry.register("CustomActionName", CustomAction);
  }}
  onUpdateHostCapabilities={(hostCapabilities: HostCapabilities) => {
    hostCapabilities.setCustomProperty("CustomPropertyName", Date.now);
  }}
  isUniqueControlInPage={true}
/>
```

- Example on use the `AdaptiveCardHost` control with SharePoint Theme:

```TSX
<AdaptiveCardHost
  card={card}
  themeType={AdaptiveCardHostThemeType.SharePoint}
  onInvokeAction={(action) => alert(JSON.stringify(action))}
  onError={(error) => alert(error.message)}
/>
```

- Example on use the `AdaptiveCardHost` control with SharePoint Theme "Section Variation" ('this.props.theme' is the theme that come from the Web Part) */):

```TSX
<AdaptiveCardHost
  card={card}
  theme={this.props.theme}
  themeType={AdaptiveCardHostThemeType.SharePoint}
  onInvokeAction={(action) => alert(JSON.stringify(action))}
  onError={(error) => alert(error.message)}
/>
```

- Example on use the `AdaptiveCardHost` control with Teams "Default" Theme:

```TSX
<AdaptiveCardHost
  card={card}
  themeType={AdaptiveCardHostThemeType.Teams}
  onInvokeAction={(action) => alert(JSON.stringify(action))}
  onError={(error) => alert(error.message)}
/>
```

- Example on use the `AdaptiveCardHost` control with Teams "Dark" Theme:

```TSX
<AdaptiveCardHost
  card={card}
  themeType={AdaptiveCardHostThemeType.TeamsDark}
  onInvokeAction={(action) => alert(JSON.stringify(action))}
  onError={(error) => alert(error.message)}
/>
```

- Example on use the `AdaptiveCardHost` control with Teams "High Contrast" Theme:

```TSX
<AdaptiveCardHost
  card={card}
  themeType={AdaptiveCardHostThemeType.TeamsHighContrast}
  onInvokeAction={(action) => alert(JSON.stringify(action))}
  onError={(error) => alert(error.message)}
/>
```

## Implementation

The `AdaptiveCardHost` control can be configured with the following properties:

| Property | Type | Required | Description |
| ---- | ---- | ---- | ---- |
| card | object | yes | Set Adaptive Card payload. |
| data | { "$root": object } | no | Set Data Source for template rendering. |
| style | React.CSSProperties | no | Set CSS Style. |
| className | string | no | Set CSS Class. |
| theme | IPartialTheme or ITheme | no | Set Fluent UI Theme. Used only if the "themeType" property is set to 'ThemeType.SharePoint'. If not set or set to null or not defined, the theme passed through context will be searched, or the default theme of the page will be loaded.  |
| themeType | ThemeType | no | Select the Type of Theme you want to use. If it is not set or set to null or undefined, the 'ThemeType.SharePoint' value will be used and the "theme" property or the theme passed through the context or default page will be loaded. In other cases, the chosen Microsoft Teams theme will be applied. |
| hostConfig | object | no | Set custom HostConfig. |
| onInvokeAction | (action: IAdaptiveCardActionResult) => void | yes | Invoked every time an Action is performed. |
| onError | (error: Error) => void | yes | Invoked every time an exception occurs in the rendering phase. |
| onSetCustomElements | (registry: CardObjectRegistry<CardElement>) => void | no | Invoked to manage Elements to the current Adaptive Card instance. |
| onSetCustomActions | (registry: CardObjectRegistry<Action>) => void | no | Invoked to manage Actions to the current Adaptive Card instance. |
| onUpdateHostCapabilities | (hostCapabilities: HostCapabilities) => void | no | Invoked to manage the HostCapabilities object like add custom properties. |
| isUniqueControlInPage | boolean | no | Set to true if you want to use only one instance of this control per page, false for multiple controls. This affects how CSS variables are set. |

Interface `IAdaptiveCardHostActionResult`

| Property | Type | Required | Description |
| ---- | ---- | ---- | ---- |
| type | string | yes | Type of the Action. |
| title | string | no | Title of the Action. |
| url | string | no | Url of the Action. |
| data | object | no | Data of the Action. |
| verb | string | no | Verb of the Action. |

Enum `AdaptiveCardHostThemeType`

| Type | Description |
| ---- | ---- |
| SharePoint | Use the SharePoint current Theme or Theme Variation |
| Teams | Use the Fluent UI Teams default theme |
| TeamsDark | Use the Fluent UI Teams dark theme |
| TeamsHighContrast | Use the Fluent UI Teams high contrast theme |

![](https://telemetry.sharepointpnp.com/sp-dev-fx-controls-react/wiki/controls/AdaptiveCardHost)
