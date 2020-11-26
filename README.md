# This is module is an abstraction to Magento 2 persistence module, it acts as a middleware that provides a high level API to connect a third marty module to Magento.

[![Latest Version on Packagist](https://img.shields.io/packagist/v/ticaje/m2-persistence.svg?style=flat-square)](https://packagist.org/packages/ticaje/m2-persistence)
[![Quality Score](https://img.shields.io/scrutinizer/g/M-Contributions/Persistency.svg?style=flat-square)](https://scrutinizer-ci.com/g/M-Contributions/Persistency)
[![Total Downloads](https://img.shields.io/packagist/dt/ticaje/m2-persistence.svg?style=flat-square)](https://packagist.org/packages/ticaje/m2-persistence)

## Preface

Magento framework is a great tool for building e-commerce solutions. It did spring out its version 2 some while ago and with it, its quality
jumped up in a tremendous way. Even so, i wanted to provide certain facilities to developers that make live easier when coding in Magento and
repetitive tasks shows up.

I must say that, and perhaps a disclaimer is lurking around, this is a series of extensions developed under S.O.L.I.D and other OO design principles
so we'd introduce some standardization in the way we develop in Magento since its designers took this way when they decided to walk a better path for the
architecture of the framework. In short, S.O.L.I.D principles and good design practices dwell all over Magento ecosystem.

## Installation


You can install this package using composer(the only way i recommend)

```bash
composer require ticaje/m2-persistence
```

## What's the fuzz about this module

This module is about Persistence.
Magento 2 came out with a profound redesign of the framework from many standpoints, it introduces modern(perhaps not so new) OO Design techniques
but if you look closer, when it comes to persistence, not greater changes has developed, it continues to shape the same ORM(if it ever had something close to an ORM)
structure, the typical Model/Resource approach remains the same.

### Problem this module solves.

It's a fact that the same boiler-plating needs to be accomplished when developer needs to create a single Entity(in Magento entities are Models).
This is way awkward because a bunch of classes(model, resources and collection) need to be created in order to create a single Entity.
It turns out that Magento provides the tools to avoid developer going through this over and over again every time a single Entity comes into play, not to talk
about creating more complex Entity relations.

This module actually refactors the way Magento does this so developer can focus on defining the proper entities and its relationships in a declarative way
simply by means of DIC a.k.a di.xml.

I will provide an example of this since an image is worth a thousand words.

### A simple example

Suppose you need to add a specific table to Magento because of your business model.
The table name is gonna be "example_table_name"; the details of the schema we're not gonna discuss here cause it continues to be same, this module does not interfere
on such a grounds.
Once developer has defined the schema(through traditional Magento channels) the only thing left to complete the classes required to glue schema with ORM is to define
the following xml within module's di.xml file.

```xml
    <virtualType name="Vendor\Module\Model\Resource\Example" type="Ticaje\Persistence\Entity\Resource\Base">
        <arguments>
            <argument name="tableName" xsi:type="string">example_table_name</argument>
            <argument name="referenceId" xsi:type="string">entity_id</argument>
        </arguments>
    </virtualType>

    <virtualType name="Vendor\Module\Model\Example" type="Ticaje\Persistence\Entity\Base">
        <arguments>
            <argument name="resourceModelClass" xsi:type="string">Vendor\Module\Model\Resource\Example</argument>
            <argument name="cacheTag" xsi:type="string">example_table_name</argument>
        </arguments>
    </virtualType>

    <virtualType name="Vendor\Module\Model\Example\Resource\Example\Collection" type="Ticaje\Persistence\Entity\Resource\Collection\Base">
        <arguments>
            <argument name="idFieldName" xsi:type="const">Vendor\Module\Model\ExampleInterface::KEY_ID</argument>
            <argument name="model" xsi:type="string">Vendor\Module\Model\Example</argument>
            <argument name="resourceModel" xsi:type="string">Vendor\Module\Model\Example\Resource</argument>
        </arguments>
    </virtualType>
```

In short, we're hereby are defining model, resource and collection classes to this new model.
So developer is empowered to use it as usually he/she does it in Magento ecosystem.

### Advantages

The benefits of using this approach is not to worry about the details of repeating over and over the same classes structure
when creating models in Magento.
The greatest benefits comes when Magento performs an upgrade in the framework and any of this classes are affected by it.
This module acts as a gateway so developer(this module's maintainer) just changes this modules to be compliant with new specifications
and consumers(modules using this module) remain unaware of it. Just picture out the case where you have many custom models defined
all over your system, you keep your self from doing a bunch of changes in many places since your logic remains Framework independent. 

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Credits

- [Héctor Luis Barrientos](https://github.com/ticaje)
- [All Contributors](../../contributors)

## License

The GNU General Public License (GPLv3). Please see [License File](LICENSE.md) for more information.
