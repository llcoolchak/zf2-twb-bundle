TwbBundle
=====================

[![Build Status](https://travis-ci.org/neilime/zf2-twb-bundle.png?branch=master)](https://travis-ci.org/neilime/zf2-twb-bundle)
![Code coverage](https://raw.github.com/zf2-boiler-app/app-test/master/ressources/100%25-code-coverage.png "100% code coverage")

NOTE : If you want to contribute don't hesitate, I'll review any PR.

Introduction
------------

__TwbBundle__ is a module for Zend Framework 2, for easy integration of the [Twitter Bootstrap](https://github.com/twitter/bootstrap). 

Demonstration / exemple
-----------------------

Render forms, buttons, alerts with TwbBundle : see it in action [on-line](http://neilime.github.com/zf2-twb-bundle/demo.html).


Requirements
------------

* [Zend Framework 2](https://github.com/zendframework/zf2) (latest master)
* [Twitter Bootstrap](https://github.com/twitter/bootstrap) (latest master)

Installation
------------

### Main Setup

#### By cloning project

1. Clone this project into your `./vendor/` directory.
2. (Optionnal) Clone the [Twitter bootstrap project](https://github.com/twitter/bootstrap) (latest master) into your `./vendor/` directory.

#### With composer

1. Add this project in your composer.json:

    ```json
    "require": {
        "neilime/zf2-twb-bundle": "dev-master"
    }
    ```

2. Now tell composer to download __TwbBundle__ by running the command:

    ```bash
    $ php composer.phar update
    ```

#### Post installation

1. Enabling it in your `application.config.php` file.

    ```php
    return array(
        'modules' => array(
            // ...
            'TwbBundle',
        ),
        // ...
    );
    ```

2. Include Twitter Bootstrap assets

###### With __AssetsBundle__ module (easy way)
    
* Install the [AssetsBundle module](https://github.com/neilime/zf2-assets-bundle)(latest master)
* Install [Twitter Bootstrap](https://github.com/twitter/bootstrap)(latest master)
* Edit the application module configuration file `module/Application/config/module.config.php`, adding the configuration fragment below:

    ```php
    return array(
        //...
         'asset_bundle' => array(
             'assets' => array(
                 'less' => array('@zfRootPath/vendor/twitter/bootstrap/less/bootstrap.less')
             )
         ),
         //...
     );
     ```

* Edit layout file `module/Application/view/layout/layout.phtml`, to render head scripts :

    ```php
    //...    
    echo $this->headScript();
    //...
    ```

###### Manually
    
* Copy `bootstrap.css` file (available on Twitter Bootstrap website(http://twitter.github.io/bootstrap/assets/bootstrap.zip)) into your assets folder and add it in your head scripts
    
# How to use __TwbBundle__

## Simple examples

* Render a dropdown button

    ```php
    //...
    //Create button
    $button = new \Zend\Element\Button('test-button',array(
        'label' => 'Action',
        'dropdown' => array('actions' => array(
            'Action',
            'Another action',
            'Something else here',
            '-',
            'Separated link'
        ))
    )));
    //Render it in your view
    echo $this->formButton($button);
    //...
    ```

* Render a search form

    ```php
    //...
    //Create form
    $form = new \Zend\Form\Form();
    $form->add(array(
        'name' => 'input-search-append',
        'attributes' => array(
            'class' => 'search-query input-medium'
        ),
        'options' => array('twb' => array(
            'append' => array(
                'type' => 'buttons',
                'buttons' => array(
                    'search-submit-append' => array(
                        'options' => array('label' => 'Search'),
                        'attributes' => array('type' => 'submit')
                    )
                )
            )
        ))
    ))->add(array(
        'name' => 'input-search-prepend',
        'attributes' => array(
            'class' => 'search-query input-medium'
        ),
        'options' => array('twb' => array(
            'prepend' => array(
                'type' => 'buttons',
                'buttons' => array(
                    'search-submit-prepend' => array(
                        'options' => array('label' => 'Search'),
                        'attributes' => array('type' => 'submit')
                    )
                )
            )
        ))
    ));
    //Render it in your view
    $this->form($form,\TwbBundle\Form\View\Helper\TwbBundleForm::LAYOUT_SEARCH);
    //...
    ```

## Features

__TwbBundle__ is abble to render [Twitter bootstrap demo site](http://twitter.github.com/bootstrap) forms, inputs, buttons, & alerts. (tests are written in order to cover what is showed on demo site)

### 1. Forms

_Render \Zend\Form\FormInterface_

#### Form layout :

Layout should be defined when form view helper is invoked

* None : `null`
* Search form : `\TwbBundle\Form\View\Helper\TwbBundleForm::LAYOUT_SEARCH`
* Inline form : `\TwbBundle\Form\View\Helper\TwbBundleForm::LAYOUT_INLINE`
* Horizontal form (default) : `\TwbBundle\Form\View\Helper\TwbBundleForm::LAYOUT_HORIZONTAL`

	Exemple : 
	    
	```php
	//...
	$this->form($form,\TwbBundle\Form\View\Helper\TwbBundleForm::LAYOUT_INLINE);
	//...    
	```

### 2. Inputs

_Render \Zend\Form\ElementInterface_

All elements options are defined in `twb` (array)

```php
new \Zend\Form\Element('test-element',array(
    'twb' => array(
        /** TwbBundle options **/
    )
);
```

#### Appended and / or prepended

For all prepended / appended types : 

```php
new \Zend\Form\Element('test-element',array(
    'twb' => array(
        'prepend' => array(
            'type' => 'prepended type',
            //Prepended type option
        ),
        'append' => array(
            'type' => 'appended type',
            //Appended type option
        )
    )
);
```

* Text :

    _Appended / prepended texts are translated_

    ```php
    //Prepended text
    new \Zend\Form\Element('test-element',array(
        'twb' => array(
            'prepend' => array(
                'type' => 'text',
                'text' => 'Prepended text'
            )
        )
    );
    ```

* Icon : 

    ```php
    //Appended icon
    new \Zend\Form\Element('test-element',array(
        'twb' => array(
            'append' => array(
                'type' => 'icon',
                'icon' => 'icon-enveloppe' //icon class
            )
        )
    );
    ```

* Button(s) :

    _Button options are explained [below](#buttons)._
 
    ```php
    //Appended buttons
    new \Zend\Form\Element('test-element',array(
        'twb' => array(
            'append' => array(
                'type' => 'buttons',
                'buttons' => array(
                    'button-one' => array(
                    	/* Button factory options, name is not mandatory if given with the array key */
                    ),
                    new \Zend\Form\Element\Button('button-two',array(/* Button options */))
                )
            )
        )
    );
    ```

* Or what ever you want :

    ```php
    //Appended markup
    new \Zend\Form\Element('test-element',array(
        'twb' => array(
            'append' => '<span>Simple appended text</span>'
        )
    );
    ```

#### Form actions

This option allows an element to be in form actions part

```php
//Element in form actions
new \Zend\Form\Element('test-element',array(
	'twb' => array(
		'formAction' => true
	)
);
```

#### Help

* Inline
    ```php
    new \Zend\Form\Element('test-element',array(
        'twb' => array(
            'help-inline' => 'Inline help text'
        )
    );
    ```

* Block
    ```php
    new \Zend\Form\Element('test-element',array(
        'twb' => array(
            'help-block' => 'A longer block of help text that breaks onto a new line and may extend beyond one line.'
        )
    );
    ```

#### Validation states

Validations states are only rendered with horizontal form layout, validation status "error" is automatically added when the element contains at least one error message.

```php
//Element with "info" state
new \Zend\Form\Element('test-element',array(
    'twb' => array(
        'state' => 'info'
    )
);
```

### 3. Buttons

Render \Zend\Form\Element\Button

#### Icons

```php
new \Zend\Form\Element\Button('test-button',array(
    'twb' => array(
        'icon' => 'icon-info'
    )
);
```

#### Button dropdowns

```php
new \Zend\Form\Element\Button('test-button',array(
    'twb' => array(
        'dropdown' => array(
            'actions' => array(
                /** action options **/
            )
        )
    )
);
```

#### Split button dropdowns

```php
new \Zend\Form\Element\Button('test-button',array(
    'twb' => array(
        'dropdown' => array(
            'segmented' => true,
            'actions' => array(
                /** action options **/
            )
        )
    )
);
```

#### Right menus

```php
new \Zend\Form\Element\Button('test-button',array(
    'twb' => array(
        'dropdown' => array(
        	'pull' => 'right',
            'actions' => array(
                /** action options **/
            )
        )
    )
);
```

#### Dropup menus

```php
new \Zend\Form\Element\Button('test-button',array(
    'twb' => array(
        'dropup' => array(
            /** dropup options (same as dropdown **/
        )
    )
);
```

#### Actions options

Should be `string` or `array`

* String : The label name (would be translated), href url is # + String value.
	Exemple : 
	```php
	//...
	'actions' => array(
	    'test' //Render <li><a href="#test">test</a></li>
	)
	//...
	```

	You can render a `divider` by using  `-` as label name

	Exemple : 
    ```php
    //...
    'actions' => array(
        '-' //Render <li class="divider"></li>
    )
    //...
    ```

* Array (available options):
    - `string` label : the label name (would be translated)
    - `string` content : markup, if `label` is defined, this option is not used
    - `string` icon : (optionnal) the icon class to prepend to label or content
    - `string` href : (optionnal) href for the link, default `#`
    - ... : all attributes you want for the link element (onclick, class...)
    
    Exemple : 
    ```php
    //...
    'actions' => array(
        array(
        	'label' => 'Test action',
        	'icon' => 'icon-user',
        	'href' => 'test.html',
        	'class' => 'test-class'
        ) // Render <li><a href="test.html" class="test-class"><i class="icon-user"></i> Test action</a></li>
    )
    //...
    ```

### 4. Alerts

_Render alerts_

Exemple : 
    
```php
//...
$this->alert('Test message','alert-error');
//...    
```

#### Params
- `string` alert message :  (would be translated)
- `string` alert class : (optionnal)
- `boolean` close : show close button or not, default true
