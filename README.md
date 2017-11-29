Cordova Unimag-Swiper Plugin
=================================

The purpose of this plugin is to allow for reading IDs using the IDTech UniMag II, UniMag Pro, or Shuttle mobile readers in hybrid applications.


I've used https://github.com/elizabethrego/cordova-plugin-unimag-swiper/ as baseline, but removed the CC parsing in order to return raw data to JSON

- __unimag__ (for UniMag original reader)
 - __unimag_ii__ (for UniMag II reader)
 - __unimag_pro__ (for UniMag Pro reader)
 - __shuttle__ (for Shuttle reader)


## Sample
The sample demonstrates how to activate the reader, capture events, and swipe a card.

```javascript
document.addEventListener("deviceready", function () { // $ionicPlatform.ready(function() {
  cordova.plugins.unimag.swiper.activate();
  cordova.plugins.unimag.swiper.enableLogs(true);
  cordova.plugins.unimag.swiper.setReaderType('unimag_ii');

  var connected = false;

  var swipe = function () {
    if (connected) {
      cordova.plugins.unimag.swiper.swipe(function successCallback () {
        console.log('SUCCESS: Swipe started.');
      }, function errorCallback () {
        console.log('ERROR: Could not start swipe.');
      });
    } else console.log('ERROR: Reader is not connected.');
  }

  cordova.plugins.unimag.swiper.on('connected', function () {
    connected = true;
  });

  cordova.plugins.unimag.swiper.on('disconnected', function () {
    connected = false;
  });

  cordova.plugins.unimag.swiper.on('swipe_success', function (e) {
    var data = JSON.parse(e.detail);
    console.log('cardholder name: ' + data.first_name + ' ' + data.last_name);
    console.log('card number:' + data.card_number);
    console.log('expiration:' + data.expiry_month + '/' + data.expiry_year);
  });

  cordova.plugins.unimag.swiper.on('swipe_error', function () {
    console.log('ERROR: Could not parse card data.');
  });

  cordova.plugins.unimag.swiper.on('timeout', function (e) {
    if (connected) {
      console.log('ERROR: Swipe timed out - ' + e.detail);
    } else {
      console.log('ERROR: Connection timed out - ' + e.detail);
    }
  });

}, false); // });
