CyberDuck-vQmods
================
This is a collection of [vQmods](http://code.google.com/p/vqmod/) for [Opencart](https://github.com/opencart/opencart) that we have developed. They have all been tested on Opencart Version 1.5.5.1

You need to [install vQmod](http://code.google.com/p/vqmod/wiki/Install_OpenCart) on your Opencart installation and then drop these files into your xml folder. Some modifications may require editing your template files.

TwitterCard
----------------
Add [Twitter Card](https://dev.twitter.com/docs/cards/types/product-card) meta information to your products.
You will need to ouput the meta data to your header.tpl 
```php
<?php
if (isset($twittercards)): foreach ($twittercards as $twittercard):
echo '<meta property="' . $twittercard['property'] . '" content="' . $twittercard['content'] . '" />';
endforeach; endif;
?>
```

OpenGraph
----------------
Add Facebook [Opengraph](https://developers.facebook.com/docs/web/tutorials/scrumptious/open-graph-object/) meta information to your products.
You will need to ouput the meta data to your header.tpl 
```php
<?php
if (isset($opengraphs)): foreach ($opengraphs as $opengraph):
echo '<meta property="' . $opengraph['property'] . '" content="' . $opengraph['content'] . '" />';
endforeach; endif;
?>
```

TransactionTracking
----------------
Add Google [Ecommerce Transaction Tracking](https://developers.google.com/analytics/devguides/collection/gajs/gaTrackingEcommerce) for completed orders.
You will need to have the Asynchronous ga.js snippet somewhere on your page and then add this to your success.tpl
```php
<?php if (isset($transaction) && !empty($transaction["id"])) { ?>
<script>
_gaq.push(['_addTrans',
  '<?php echo $transaction["id"];?>',        // transaction ID - required
  'Cyber-Duck',                              // affiliation or store name
  '<?php echo $transaction["total"];?>',     // total - required; Shown as "Revenue" in the
  '<?php echo $transaction["tax"];?>',       // tax
  '<?php echo $transaction["shipping"];?>',  // shipping
  '<?php echo $transaction["city"];?>',      // city
  '<?php echo $transaction["state"];?>',     // state or province
  '<?php echo $transaction["country"];?>'    // country
]);

<?php 
foreach ($transaction['products'] as $product) { ?>
_gaq.push(['_addItem',
    '<?php echo $transaction["id"];?>',      // transaction ID - required
    '<?php echo $product["model"];?>',       // SKU/code - required
    '<?php echo $product["name"];?>',        // product name
    '',                                      // category or variation
    '<?php echo $product["price"];?>',       // unit price - required
    '<?php echo $product["quantity"];?>'     // quantity - required
  ]);
<?php } ?>

_gaq.push(['_trackTrans']);
</script>
<?php } ?>
```