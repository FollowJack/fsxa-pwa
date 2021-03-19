## Configuration

### .env-File

The .env file allows you to provide required variables for the local build process. We recommend that you do not check them into your source code management. Ideally, you inject them later via your CI/CD process. If you are already using the CCD pipeline from e-Spirit, we will do this for you fully automatically.

All of the following variables are required.

#### `FSXA_API_KEY` `string`

This API key is required by the PWA to obtain authorized access to the CaaS and navigation service. Our Customer Success Management team (insert mailto link here) will gladly provide you with this key after the successful provisioning of your FSXA environment.

#### `FSXA_CAAS` `string`

The URL under which the CaaS can be reached. This URL is only used on the server and is not visible to the user in the client.

#### `FSXA_NAVIGATION_SERVICE` `string`

The URL under which the navigation service is available. This URL is only used on the server and is not visible to the user in the client.

#### `FSXA_PROJECT_ID` `string`

Several projects can be configured on your FirstSpirit server. In order for the PWA to know which data it needs to access, it needs the UUID of the relevant project.

> TODO: Should we elaborate a bit more on how to find the project id?

#### `FSXA_TENANT_ID` `string`

The tenantId is used by the PWA to distinguish between the different FSXA environments provided to you.

#### `FSXA_MODE` `"preview" | "release"`

We distinguish between preview and release data. Specify here which data should be loaded by the PWA.

### fsxa.config

The following settings allow easy configuration of the PWA.

#### General

##### `devMode`

The `DevMode` helps you to quickly build new components or adapt existing ones.

> TODO: Add screenshots and explain this a little more

##### `defaultLocale`

Normally the current URL is used to find out the language. If this is not possible (for example, when a user calls `/`) the `defaultLocale` is used to retrieve the navigation data via the `FSXA API`. This property is required.

#### Components

The components are automatically loaded and mapped.
It is important to note the following aspects:

File names should be defined in camelCase. These are subsequently converted to snake_case. Folders are also mapped and separate the converted identifier with a `.`. The exception here is the `index` file. This leads to the following mapping:

- `teaser.tsx` &#8594; `teaser`
- `Teaser/index.vue` &#8594; `teaser`
- `NewsTeaserSection.tsx` &#8594; `news_teaser_section`
- `InterestingFacts/index.tsx` &#8594; `interesting_facts`
- `InterestingFacts/SubComponent.tsx` &#8594; `interesting_facts.sub_component`
- `InterestingFacts/components/Counter.vue` &#8594; `interesting_facts.components.counter`

You can override this automatism by creating an `index.ts` / `index.js` in the configured folder.

```javascript
import MyCustomTeaserComponent from '...'
import Counter from '...'

export default {
  teaser: MyCustomTeaserComponent,
  'interesting_facts.components.counter': Counter
}
```

#### The folders configured by the following settings are automatically searched for files with the extensions `.vue`, `.tsx`, `.jsx` and `.ts`.

##### Sections

`components.sections` - optional

The folder, where all your section components are located.

> We recommend you to derive from [FSXABaseSection](components/FSXABaseSection.md) to get access to useful functionality and add TypeScript support.

**Default**: `"~/components/fsxa/sections"`

##### Layouts

`components.layouts` - optional

The folder, where all your layout components are located.

> We recommend you to derive from FSXABaseLayout to get access to useful functionality and add TypeScript support.

**Default**: `"~/components/fsxa/layouts"`

##### RichText

`components.richtext` - optional

The folder, where all your richtext components are located.

**Default**: `"~/components/fsxa/richtext"`

> We recommend you to derive from FSXABaseRichTextElement to get access to useful functionality and add TypeScript support.

##### AppLayout

`components.appLayout` _optional_

You have the option to specify an AppLayout component that is rendered as a global wrapper around your mapped content.

This setting is optional. **Default**: `undefined`

> We recommend you to derive from FSXABaseAppLayout to get access to useful functionality and add TypeScript support.

##### Loader

`components.loader` _optional_

**Default**: `undefined`

##### Custom Routes

`customRoutes` _optional_

This setting configures the folder in which your own endpoints are located. The automatism loads files with the extensions ts and js. You can learn more in the section [CustomRoutes](advanced/CustomRoutes.md).

> These settings can be configured in the following ways:

#### fsxa.config.ts / fsxa.config.js

```typescript
{
  devMode: true,
  defaultLocale: "de_DE"
  // each of the entries is optional
  // default values mentioned above will be used
  components: {
    sections: "~/components/fsxa/sections",
    layouts: "~/components/fsxa/layouts",
    richtext: "~/components/fsxa/richtext",
  }
}
```

#### nuxt.config

```typescript
{
  ...,
  fsxa: {
    devMode: true,
    defaultLocale: "de_DE"
    // each of the entries is optional
    // default values mentioned above will be used
    components: {
      sections: "~/components/fsxa/sections",
      layouts: "~/components/fsxa/layouts",
      richtext: "~/components/fsxa/richtext",
    }
  },
  ...
}
```