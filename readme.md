#Barcode JS: Shopify Order Printer App

* [Before you get started](#before-you-get-started)
* [Setup instructions](#setup-instructions)
* [Options](#options)
* [Acknowledgments](#acknowledgments)

This is a JavaScript solution to have Shopify's [Order Printer app](https://apps.shopify.com/order-printer) 
display product variant barcodes in the following formats:

* [CODE39](http://en.wikipedia.org/wiki/Code_39)
* [CODE128](http://en.wikipedia.org/wiki/Code_128)
* [EAN/UPC](http://en.wikipedia.org/wiki/International_Article_Number)

Below is a screenshot of a basic template using this barcode solution:

![Barcode JS in action](http://snapify.shopify.com/12-14-sbn1g-fftu5.png)


##Before you get started

These instructions assume you have already entered barcodes for your product variants.  Its not
necessary that all your products have this field filled out, but that is the piece of data these
instructions will target.  To assign a barcode to a variant, head to that product's page from your store's
dashboard and Edit the individual variants.

![Variant editing screenshot](http://snapify.shopify.com/11-56-8bsrm-3mpqq.png)

###Considerations

This javascript solution can turn a barcode string into a scannable format, provided that the string you
are converting can create a **valid barcode**.  

Strings can be converted into the CODE128 fairly easily.
However, CODE39 will not accept lowercase letters - so if you want to use this format, you will have to enter the 
variant's barcode appropriately.  EAN/UPC format will only accept numbers, and the string must follow a certain
convention.

###Other uses
You aren't limited to using this javascript solution on just variant barcodes, but there are limitations
as to what strings can be turned into different barcode formats.

##Setup instructions

###Get the barcode.js file

Download this [barcode.js](https://rawgit.com/NathanPJF/shopify-barcode-js/master/barcode.js.zip) file and upload it to your Shopify store's *Files directory*.  It is strongly recommended that 
it is uploaded in the store Files directory as opposed to an individual theme's Assets directory.

Make note of the file's URL as you will need to copy/paste this later.

![File URL screenshot](http://snapify.shopify.com/11-37-um2o2-g46yz.png)

###Create a new template in Order Printer

*Note*: The following will only present the HTML and liquid text needed to display barcodes.  If you would like more information
on your template, such as shipping information or product photos, I recommend you copy and paste the code from a template
you already like.

Add a template to your Order Printer app and title it.  In the first couple lines, add the following lines of code:

```
<script>
[
 '//code.jquery.com/jquery-1.11.2.min.js',
 'url-of-uploaded-barcode.js'
].forEach(function(src) {
  var script = document.createElement('script');
  script.src = src;
  script.async = false;
  document.head.appendChild(script);
});

</script>
```

Replace the text "url-of-uploaded-barcode.js" with the URL of the barcode.js file you uploaded to your Files directory.  The
URL must remain within the single-quotes.

Later in your template, make a liquid reference to the product variant barcode within a [line_item](http://docs.shopify.com/themes/liquid-documentation/objects/line_item) forloop.  Below is the basic mark-up:

```
{% for line_item in line_items %}
  <p>{{ line_item.name }}</p>
  {% unless line_item.variant.barcode == blank %}
    <p class="barcode-section"> <canvas class="item_barcode" data-barcode="{{ line_item.variant.barcode }}"></canvas></p>
  {% else %}
    <em>No barcode for this item</em>
  {% endunless %}
<hr>
{% endfor %}
```

###Required markup

The barcode must have the attributes `class="item_barcode" data-barcode="{{ line_item.variant.barcode }}"` within the element in order for 
the barcode.js functions to work.

The attributes can be within a `<canvas>` tag or an `<img>` tag.  Those are your only options for HTML elements.

##Options

You can add a number of attributes which will change the look of the barcode:

* *data-format: string* - Scannable format of barcode. Options: CODE39, CODE128, EAN. Default: CODE128
* *data-width: integer* - Width of scannable bars. Default: 1
* *data-height: integer* - Height (in pixels) of scannable bars. Default: 50
* *data-display: boolean* - Display barcode string under the scannable area. Default: false
* *data-font: string* - Websafe font-family of displayed barcode string. Default: Monospaced
* *data-fontsize: integer* - Font size (in pixels) of barcode string. Default: 16

###Examples

####Minimum markup:

```
<canvas class="item_barcode" data-barcode="{{ line_item.variant.barcode }}"></canvas>
```
![Basic barcode markup output](http://snapify.shopify.com/12-23-a4oma-bm307.png)

####Specified height and width; display barcode string
```
<canvas class="item_barcode" data-barcode="{{ line_item.variant.barcode }}" data-height="100" data-width="2" data-display="true"></canvas>
```
![Tailored barcode output](http://snapify.shopify.com/12-22-h43f9-9d64p.png)

####Change format; barcode string font-family, font-size
```
<canvas class="item_barcode" data-barcode="{{ line_item.variant.barcode }}" data-format="CODE39" data-font="sans-serif" data-fontsize="22" data-display="true"></canvas>
```
![CODE39 example](http://snapify.shopify.com/12-28-hyi5x-0tkqq.png)


##Acknowledgments

This solution is based heavily on the work done by [lindell's JsBarcode repo](https://github.com/lindell/JsBarcode)

As per [lindell's Read Me instructions](https://github.com/lindell/JsBarcode/blob/master/MIT-LICENSE.txt):

Copyright (c) 2012 Johan Lindell (johan@lindell.me)

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.


