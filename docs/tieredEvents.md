# Tiered Events

> Tiered events do have to be configured through SLOT, please ask your product manager to configure this before starting the implementation of this feature.

A tiered event is a unique event(not to be confused with tracking events) which allows the user to get special rewards by spending currencies or items within the game. A tiered event consists of multiple tiers, each individual tier has a certain goal. The goal is always to spend a certain amount of currency or items and the player will get a reward after completing a tier and then progresses to the next tier. The tiered event is finished when all the tiers are completed or when the end date of the event has been reached. The player has to spend currencies or items depending on the goal of the current tier he’s in, to progress. A web view is used to visualize the progress of the user which can be accessed through a button in the games GUI. The reward(s) for the current tier can be claimed through this web view when the goal has been reached by the player.
It’s possible to configure multiple Tiered Events to be able to run at the same time.

## Using tiered events

The tiered events configuration can be requested manually after the Gamedock SDK has been initialized and either the available or not available callback will be fired in order to indicate if any tiered events are available:

~~~C#
// Request
Gamedock.Instance.RequestTieredEvents();

// Available
Gamedock.Instance.TieredEventsCallbacks.OnTieredEventsAvailable -= OnTieredEventsAvailable;
Gamedock.Instance.TieredEventsCallbacks.OnTieredEventsAvailable += OnTieredEventsAvailable;
void OnTieredEventsAvailable(){}

// Not available
Gamedock.Instance.TieredEventsCallbacks.OnTieredEventsNotAvailable -= OnTieredEventsNotAvailable;
Gamedock.Instance.TieredEventsCallbacks.OnTieredEventsNotAvailable += OnTieredEventsNotAvailable;
void OnTieredEventsNotAvailable(){}
~~~

To get a list of all available active tiered events use the following method:

~~~C#
List<TieredEvent> activeTieredEvents = GetAllTieredEvents();
~~~

To get the progress of a tiered event call:

~~~C#
TieredEventProgress progress = GetTieredEventProgress(int tieredEventId);
~~~

## Updating tiered events

The progress is automatically updated by the Gamedock SDK when either currency is being spent using the Wallet feature or when items are being spent using the Inventory feature.
An event is fired to inform that the tiered event has been updated:

~~~C#
Gamedock.Instance.TieredEventsCallbacks.OnTieredEventUpdated -= OnTieredEventUpdated;
Gamedock.Instance.TieredEventsCallbacks.OnTieredEventUpdated += OnTieredEventUpdated;

void OnTieredEventUpdated(TieredEventProgress progress) {
  progress.tieredEventId; // The id of the tiered event which was updated
  progress.currentTierId; // The id of the current active tier within the tiered event
  progress.currentAmount; // The progress within the current tier
  progress.completed; // Indicates if the tiered event is completed
  progress.completedTiers; // List with completed tier ids
  progress.claimableTiers; // List of tier ids which are claimable by the player
} 
~~~

## Showing tiered event progress

The progress of the tiered event is visible to the user through a web view which is controlled by the Gamedock SDK and can be opened using a single call. There are 2 events which will be called to inform if the screen is opened or being closed:

~~~C#
// Show the progress screen
Gamedock.Instance.ShowTieredEventProgress(int tieredEventId);

// Open
Gamedock.Instance.TieredEventsCallbacks.OnTieredEventProgressOpen -= OnTieredEventProgressOpen;
Gamedock.Instance.TieredEventsCallbacks.OnTieredEventProgressOpen += OnTieredEventProgressOpen;
void OnTieredEventProgressOpen() {}

// Closed
Gamedock.Instance.TieredEventsCallbacks.OnTieredEventProgressClosed -= OnTieredEventProgressClosed;
Gamedock.Instance.TieredEventsCallbacks.OnTieredEventProgressClosed += OnTieredEventProgressClosed;
void OnTieredEventProgressClosed() {}
~~~

If the user claims his reward in the web view the OnTieredEventUpdated event will be called with the new active tier information.

## Handling errors

A few errors can occur for the tiered events feature, the following event is used to inform you:

~~~C#
Gamedock.Instance.TieredEventsCallbacks.OnTieredEventsError -= OnTieredEventsError;
Gamedock.Instance.TieredEventsCallbacks.OnTieredEventsError += OnTieredEventsError;
void OnTieredEventsError(GamedockErrorMessage error) {
}
~~~

The following errors are possible:
* Id 42, TieredEventShowProgressError: The progress of the player could not be shown.
* Id 43, TieredEventUpdateProgressError: The progress of the tiered event failed to update.
* Id 44, TieredEventClaimTierError: The user tried to claim his reward but this failed.