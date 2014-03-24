# Generalized Wallets Scheme - Merchant Integration

This document's purpose it to match as close as possible the expectations of merchants. It may sometimes play the devil's advocate.

Let's review the steps of a standard purchase, and for each one, see what could be improved and where standards could help.

## Shopping cart contents

Some payment services allow the merchant to provide them the shopping cart contents.

Pros:

- If the customer is redirected to another website, he/she will probably feel more comfortable to see that everything looks consistent.
- It provides some valuable data for fraud detection.

Cons:

- Consistency must be perfect.
- It raises some privacy concerns. I'm not sure that a customer would like to have all his/her purchase details handed out eventually to his/her bank.

## Billing and Shipping information

These are in most cases the most annoying forms to fill out.

Unlike the shopping cart which is linked to the purchase, and temporary, these are linked to Identity.
Identity is a whole subject in itself, but we feel that standardization on that subject would benefit both the customer and the merchant.

Well, at least when the transaction is concluded, as it raises privacy concerns as well. Address is a valuable information to compute taxes and shipping fees, so having this information early in the purchase process is a good thing. However, coupled with device fingerprinting et al, it could be harmful.

In fact, as in the physical world, the merchant wants the customer to open his wallet as soon as possible, which is probably not what the customer wants.

## Payment method(s) selection

This deserves a special attention, as it can work both ways:

- The merchant could provide a list of supported payment methods, for the wallet to filter.
- Or the wallet could provide this list to the merchant.

It would be a huge benefit for the merchant if he was able to know in advance which payment methods are available.
Note that it is not necessarly to restrict the choice of these payment methods, as some of them propose an enrollment scheme. Examples: credit offers, wallets. Revenue sharing is likely to be involved in this case.

## Payment method(s) entry

Mostly security concerns here. Note that even if the merchant doesn't perform card details entry (and he shouldn't, in my opinion), he is still interested in getting at least some acquirer-related information, for banking fees optimization.

## 3-D Secure and other authentication schemes involving an issuer which is not the acquirer

This is the real pain point here, for both the merchant and the customer.

In the case of 3-D Secure for instance, it is seen as a poor user experience, due to the "context switching" from the merchant's website (the merchant often tries to make the purchasing process as smooth as possible) to an issuer website where no inherited elements are transmitted.

Then there is the authentication process itself. Many attempts have been made on that subject, even not-so-clever ones (birthday).

It's hard to know the state of the art of the different authentication systems, but some are quite common.

Standardization could be envisioned in small steps, by taking most common use cases one by one. 
Examples:

- SMS OTPs could be standardized, which could lead to automatic OTP entry while browsing from your mobile phone.
- Authentication forms provided by the issuer could be enhanced, using a standard allowing merchant-provided assets to be displayed on them, in order to avoid the "context-switching" issue.

## Digital Receipts

There are (iat least) two kinds of receipts:

- The merchant-supplied receipt. How many in the case of marketplaces?
- The payment gateway supplied receipt. One at least per payment method.

The first type of receipt seems inappropriate for standardization, as its role is not limited to providing plain information about the payment.
It can be used as a support for loyalty, future sales, donation fiscal receipt etc.

The payment gateway provided receipt, however, could be standardized. Maybe as a multipart message, with a human-readable part, and a machine-readable part.

This receipt should be digitally signed!
