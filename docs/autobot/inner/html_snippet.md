# Integrating Bot onto App

Once you have your bot iframe, we make it *super* easy to integrate your bot anywhere. 

The script will look something like:
```html
<script type="text/javascript"> function resizeCrossDomainIframe(id) { var iframe = document.getElementById(id); iframe.height = 70 + "px"; iframe.width = 70 + "px"; window.addEventListener('message', function(event) { var height = parseInt(event.data); if (height === 48) { iframe.height = height + 22 + "px"; iframe.width = height + 22 + "px"; } else { iframe.height = height + "px"; iframe.width = "400px"; } }, false); } </script> <iframe id="d4696471-2b82-4b76-ae7c-ac9e65b3c885" src="https://bot.autessa.com/d4696471-2b82-4b76-ae7c-ac9e65b3c885" style="z-index:999;border-radius: 5px;overflow: hidden;border: none;position:fixed;bottom: 0;right: 0;" onload="resizeCrossDomainIframe('d4696471-2b82-4b76-ae7c-ac9e65b3c885');"></iframe>
```

## WordPress
On wordpress, make sure to make your site "plugin-enabled". Sometimes the script option does not play well with HTML, so make sure to select the "no-script" option for the snippet to embed if you have issues.

To add an embed in WordPress, follow this [tutorial](https://wordpress.com/support/wordpress-editor/blocks/custom-html-block/#add-your-code)

## Wix
On Wix, script tags sometimes don't play well, so make sure to select the "no-script" option for the snippet to embed.

To add an embed in Wix, you will need to add a [custom HTML element](https://support.wix.com/en/article/wix-editor-embedding-a-site-or-a-widget#adding-an-embed).

## GitHub Pages
In GitHub pages, simply update your `index.html` to contain the bot. To have the bot appear on every page, we recommend adding it to the `<head>` section on your index.html.

For example, in our documentation, we added it [here](https://github.com/Autessa-Inc/AutessaPublicDocumentation/blob/main/docs/index.html#L94) 
