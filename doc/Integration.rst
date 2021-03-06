Integration
===========

Doctrine integration
--------------------

Independently from your preferred framework's characteristics, it is also possible to use plain
Doctrine with the Money VO, using its Embeddables_ feature.

.. _Embeddables: http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/tutorials/embeddables.html

Since you should not use annotations on the third party's library files, you'll use the `YAML mapping`_.

.. _YAML mapping: http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/reference/yaml-mapping.html#yaml-mapping

The following two yaml files for Money and Currency turned out working well:

.. code-block:: yaml

    Money\Money:
      type: embeddable
      table: money # the persistence table name you'd like to use

      embedded:
        'currency':
          class: '\Money\Currency'

      fields:
        'amount':
          type: integer
          nullable: false
          options:
            unsigned: true

.. code-block:: yaml

    Money\Currency:
      type: embeddable
      table: money_currency # the persistence table name you'd like to use

      fields:
        'name':
          type: string
          nullable: false
          length: 3

Symfony configuration
^^^^^^^^^^^^^^^^^^^^^

For convenience, see the following configuration which would work out of the box for your
Symfony2 stack:

.. code-block:: yaml

    doctrine:
        orm:
            entity_managers:
                default: #or whatever fits in your case
                    mappings:
                        'Money':
                            type: yml
                            # find the above mentioned files Money.orm.yml and Currency.orm.yml
                            # in app/config/persistence/Money/ :
                            dir: '%kernel.root_dir%/config/persistence/Money'
                            # The PHP namespace prefix:
                            prefix: 'Money'
                            is_bundle: false
