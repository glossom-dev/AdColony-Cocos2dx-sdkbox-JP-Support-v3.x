##API Reference##

###Methods###

- **static void init();**
	
    initialize the plugin instance.

- **static int zoneStatusForZone(const std::string& zoneID);**

    returns the status for the specified zone. Use this to pre-load a zone.

- **static void show(const std::string& name);**

    play video ad using provided **zone name **that was specified in sdkbox_config.json.

- **static void setListener(AdColonyListener\* listener);**

    set a listener to listen for event changes.

- **static void removeListener();**

    remove the event listener.

- **static bool isVirtualCurrencyRewardAvailableForZone(const std::string& zoneID);**

    check if this zone offers a virtual currency reward.

-   **static int getVirtualCurrencyRewardsAvailableTodayForZone(const std::string& zoneID);**

    is there a virtual currency reward available to the user today for passed in zone.

- **static std::string getVirtualCurrencyNameForZone(const std::string& zoneID);**

    get virtual currency name for passed in zone.

- **static int getVirtualCurrencyRewardAmountForZone(const std::string& zoneID);**

    the amount of the virtual currency reward for passed in zone.

- **static int getVideosPerReward(const std::string& currencyName);**

    are there multiple videos to watch per reward? Get the number of them.

- **static int getVideoCreditBalance(const std::string& currencyName);**

    get video credit balance for passed in currency name.

- **static void cancelAd();**

    stop the currently showing ad.

- **static bool videoAdCurrentlyRunning();**

    is there a video currently showing?

- **static void turnAllAdsOff();**

    turn off all ads.


###Listeners###

- **void onAdColonyChange(const std::string& zoneID, bool available);**

- **void onAdColonyReward(const std::string& zoneID, const std::string& currencyName, int amount, bool success);**

    reward was received.

- **void onAdColonyStarted(const std::string& zoneID);**

    showing an ad has started.

- **void onAdColonyFinished(const sdkbox::AdColonyAdInfo& info);**

    showing an ad has finished.