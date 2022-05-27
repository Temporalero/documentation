=====
Ogone
=====

`Ogone <https://www.ingenico.com/>`_, also known as **Ingenico Payment Services** is a France-based
company that provides the technology involved in secure electronic transactions.

Configuration
=============

.. note::
   - For more extensive documentation, you can refer to Ogone's `own documentation
     <https://epayments-support.ingenico.com/en/get-started/>`_.
.. seealso::
   - :ref:`payment_acquirers/add_new`

Settings in Ogone
-----------------

Create an API user
~~~~~~~~~~~~~~~~~~

Log into your Ogone account and head to the :guilabel:`Configuration` tab.

You need to create an **API user** that will be used to create transactions from Odoo. While you can
use your main account to do so, using an **API user** ensures that if the credentials used in Odoo
are leaked, no access to your Ogone configuration will be possible. Additionally, passwords for
**API users** do not need to be updated regularly, unlike normal users.

To create an **API user**, go to :menuselection:`Configuration --> Users` and click on **New User**.
The following fields must be configured:

- **UserID:** you can choose anything you want, the convention is to name it *OOAPI*
- **User's Name, E-mail and Timezone:** you can enter the information you want
- **Profile:** should be set to *Admin*
- **Special user for API:** should be checked

Upon creation of the user, you will be required to generate a password. Save the password and
**UserID**, as they will be required later on during the setup.

.. tip::
   - Use a password manager to generate a strong password and store it more easily.

Set up Ogone for Odoo
~~~~~~~~~~~~~~~~~~~~~

Ogone must now be configured to accept payments from Odoo. Head to :menuselection:`Configuration -->
Technical Information --> Global Security Parameters`, select **SHA-1** as **Hash Algorithm** and
**UTF-8** as **character encoding**. Then, go to the **Data and Origin Information** tab of the same
page and leave the URL field of the **e-Commerce and Alias Gateway** section blank.

You are now required to generate a **SHA-IN** passphrase. **SHA-IN** and **SHA-OUT** passphrases are
used to digitally sign the transaction requests and responses between Odoo and Ogone. By using these
secret passphrases and the **SHA-1** algorithm, both systems can ensure that the information they
receive from the other was not altered or tampered with.

.. important::
   - If you need to use another algorithm, such as **SHA-256** or **SHA-512**, within Odoo, enable
     **Debug Mode** and go to :menuselection:`General Settings --> Technical -->System Parameters`.
     From here, search for **payment_ogone.hash.function** and change the value line to the desired
     algorithm (**SHA-256** or **SHA-512**).

Your **SHA-IN** and **SHA-OUT** passphrases should be different, and between 16 and 32 characters
long. Make sure to use the same **SHA-IN** and **SHA-OUT** passphrases throughout the entire Ogone
configuration, as Odoo only allows a single **SHA-IN** and single **SHA-OUT** passphrase. In the
**DirectLink and Batch** section of the screen, add the same **SHA-IN** passphrase as the previous
field. You can leave the IP address field blank.

When done, head to :menuselection:`Configuration --> Technical Information --> Transaction Feedback`
and check the following options:

- **I would like to receive transaction feedback parameters on the redirection URLs:** should be
  checked
- **Direct HTTP server-to-server request:** should to be set to *Online*
- Both **URL** fields should contain the same following URL:
  *https://<your-odoo-instance-url>/payment/ogone/accept*
- **Request Method:** should be set to *Post*
- **Dynamic eCommerce Parameters** should contain contain the following values: *ALIAS*, *AMOUNT*,
  *CARDNO*, *CN*, *CURRENCY*, *IP*, *NCERROR*, *ORDERID*, *PAYID*, *PM*, *STATUS*, *TRXDATE*. Other
  parameters can be included (if you have another integration with Ogone that requires them), but
  are not advised.
- The **URL** fields for **HTTP redirection in the browser** can be left empty, as Odoo will specify
  these URLs for every transaction request.
- In the **All transaction submission modes** section, fill in **SHA-OUT** passphrase and disable
  **HTTP request for status change**.

.. note::
   - If you use tokenization (allowing users to save their cards), go to
     :menuselection:`Configuration --> Alias` and set up the tokenization process according to your
     needs.

.. important::
   - If you wish to run tests with Ogone, change the **State** of the Test Account to *Test Mode*.
     We recommend doing this on an Odoo test database, rather than on your main database.

Settings in Odoo
----------------

Credentials
~~~~~~~~~~~

To set up Ogone in Odoo, head to :menuselection:`Accounting --> Configuration --> Payment Acquirers`
or :menuselection:`Website --> Configuration --> Payment Acquirers` and open the Ogone acquirer. In
the :guilabel:`Credentials` tab, enter the following information:

- **PSPID:** The **PSPID** of your Ogone account
- **API User ID:** Your **API user**'s ID (e.g. *OOAPI* if used conventionally)
- **API User Password:** The password of the **API user**
- **SHA Key IN/OUT:** The **SHA-IN** and **SHA-OUT** passphrases set in the backend of Ogone

Optional configuration
~~~~~~~~~~~~~~~~~~~~~~

Under the :guilabel:`Configuration` tab, you can check the option **Allow Saving Payment Methods**.
This option allows a customer to save their payment's details for future purchases, but *only* works
if you have configured the tokenization under :menuselection:`Configuration --> Alias` in Ogone's
backend.

Your Ogone account is now ready to be used with Odoo!