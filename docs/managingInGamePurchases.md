# Managing In-Game Purchases

In-app purchases refer to items or points that a player can buy for use within a game to improve a character or enhance the playing experience. These, together with the use of smart ads (described in “Smart Advertisements Support”) are the primary means by which games produce revenue for their makers. The Gamedock platform supports near real-time price changes and timed promotions.

## Understanding the in-app Purchasing Strategy

It is important to understand that within the Gamedock platform in-app purchases cannot be added on-the-fly or solely through the appropriate game’s store. The following strategy is used:

1. The game producer should define various price packages with your game store submission. For example, Starter Pack A for $0.99, Starter Pack B for $1.99, and so on. The contents of each package (for example, the number of coins that the user receives) should also be defined.
1. Your Gamedock Account Manager will then coordinate with Gamedock LiveOps to ensure that the promotions, and the packages associated with them, are set up on the Gamedock server. Typically, this includes any discounts that users should receive as part of the promotions, and the period during which specific promotions should be available.
1. Using the Gamedock SDK, developers can request information from SLOT about the active promotions and packages to implement and manage the in-game shop.

## Retrieving IAP Packages Information

The IAP Packages feature within the Gamedock SDK can automatically request IAP package information when the game starts. To retrieve this information, use the following code:

~~~C#
// Retrieve Packages from the SDK (locally)
PackagesHelper helper = Gamedock.Instance.GetPackages ();

// The PackagesHelper class contains helper methods.
// It contains a list "Packages" which have all the information required.
List<Package> Packages;

// Request package by Store ID.
Package package = helper.GetPackageByPackageId(string packageId);

// Request package by SLOT ID.
Package package = helper.GetPackageById(string id);

// Check package has a promotion.
package.HasActivePromotion()

//Informs that the packages request has been successful
Gamedock.Instance.PackagesCallbacks.OnPackagesAvailable -= OnPackagesAvailable;
Gamedock.Instance.PackagesCallbacks.OnPackagesAvailable += OnPackagesAvailable;

//Informs that packages could not be retrieved from the server
Gamedock.Instance.PackagesCallbacks.OnPackagesNotAvailable -= OnPackagesNotAvailable;
Gamedock.Instance.PackagesCallbacks.OnPackagesNotAvailable += OnPackagesNotAvailable;
~~~

## IAP Tools/Libraries

The Unity IAP library is the standard library used by most of our developers. If you’d like to use a different IAP library then that’s fine, but please confirm this with your Gamedock representative.