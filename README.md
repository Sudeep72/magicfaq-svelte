
---

# @magicfaq/svelte

A Svelte component for embedding the Magic FAQ widget.

## Installation

To install the Magic FAQ component for Svelte, run:

```sh
npm install @magicfaq/svelte
```

or

```sh
yarn add @magicfaq/svelte
```

## Usage

In your Svelte component, import and use the `MagicFAQ` component like this:

```svelte
<script lang="ts">
  import MagicFAQ from '@magicfaq/svelte';
</script>

<MagicFAQ uid="your-unique-id" position="top-right" />
```

> Remove if you have any globally  
`color-scheme: light dark`


## Props

| Prop      | Type   | Description                                                                                   |
|-----------|--------|-----------------------------------------------------------------------------------------------|
| `uid`     | string | The unique ID of the Magic FAQ widget. This is required for the component to function properly. |
| `position` | string | The position of the widget on the screen. Can be `top-left`, `top-right`, `bottom-left`, or `bottom-right`. Default is `bottom-right`. |

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Support

For support with the Magic FAQ widget, please visit our [support page](https://sudeepdev.co).

---