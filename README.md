# PadOpt

## Description

PadOpt is a free module for ProcessWire (PW), designed to improve e-shops based on PadLoper. Once PadOpt is installed along PadLoper, it is easy to define dozen of options (free or not) with dozen of choices, for every product.

PadLoper is a commercial module for (PW). PadLoper enables to transform any PW-based website to an e-shop, quickly and easily. Every PW page can be transform into a product, with a price defined in a PW field. If the product presented in the page is available with differents attributes (eg. colors, sizes), each possible combination has to be defined as a variation of the page in PW. But with several choices to do, with several possibilities for each one, the number of variations grows exponentially. For this reason, it is not really possible to natively make a shop with high customizable products with PadLoper. PadOpt fixes this issue.

## Installation

Requirements:

- ProcessWire v3.0
- PadLoper v1.3

Installation:

1. Install PadOpt like any other PW module
2. Create or modify a *product template* for PadLoper (see settings of PadCart), and add the *padopt_tpl_id* field
3. You now can create new high customizable products, or update already existing ones to complete them with some options

## How to Define Options of a Product

Let's take the classical example of the shirts, with these options:

- 3 colors available (extra charge of 10€ for the golden, the two other ones are free)
- free text area to print a name on the back (extra charge of 15€)
- possibility to ask for a gift paper (extra charge of 5€)

Here are the different steps:

1. Create a new template named *padopt_shirts*
2. Create 3 new fields:
  - *padopt_input_shirtColor* (type *Select Options*)
  - *padopt_input_shirtName* (type *Text*)
  - *padopt_input_giftPaper* (type *Checkbox*)
3. Add these 3 new fields to the new template
4. Create a new page associated to a *product template* (see Installation) and select *padopt_shirts* when asked (and set a price, just like any other products)

Respect the prefixes (*padopt_* and *padopt_input_*) in the names. To define the price of the paid options, juste include it in the label of the field, or the title of the choice, following this example: "*Golden (+ 10€)*".

You can now make an order: a list of the selected options is available directly in your *Padloper Orders* section and the final prices include the price of the options.

## How to Better Integrate PadOpt

With PadLoper, you are encouraged to customize the templates by copying them from *site/modules/PadLoper/templates/* to *site/templates/padloper/*.

With PadOpt, everywhere there is a *foreach* with PadLoper products, you can use inside:

```
list($padopt_url, $padopt_list, $padopt_price) = $modules->get('PadOpt')->getCartProductOptionsInfos($p);
```

- **$p**: PadLoper product (eg. as in *foreach($items as $p)*);
- **$padopt_url**: URL to show the selected options directly from the product page;
- **$padopt_list**: HTML list of the chosen options for this product;
- **$padopt_price**: Total price of the options (already included in the final price of the product) for this product;

Templates to update:

- To show options in carts, update *edit-cart.php*.
- To show options in order confirmations and invoices, update *order-products-table.php*.

## PadOpt Submodules

PadOpt supports a submodule system, to easily add functionnalities.

To create a new submodule, just create a module with a class inheriting *PadOptSubmodule* and register yourself with this line in *init()*:

```
$this->modules->get('PadOpt')->registerSubmodule($this);
```

### Live Customizers Submodule



## How it Works Behind the Scenes

PadOpt adds two main things to the PadLoper stuff:

1. A column named *padopt_options* in the MySQL table named *padcart*
2. A PW field named *padopt_options* associated to the *padorder_product* template

The db column stores the options associated to the corresponding product in the cart, in PHP's serialization format. The PW field does the same thing, but when the cart is tranform into an order.

All other changes are done on the fly with hooks.
