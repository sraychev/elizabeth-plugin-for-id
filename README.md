Cordova Unimag-Swiper Plugin
=================================

The purpose of this plugin is to allow for reading IDs using the IDTech UniMag II, UniMag Pro, or Shuttle mobile readers in hybrid applications.


I've used https://github.com/elizabethrego/cordova-plugin-unimag-swiper/ as baseline, but removed the CC parsing in order to return raw data to JSON

- __unimag__ (for UniMag original reader)
 - __unimag_ii__ (for UniMag II reader)
 - __unimag_pro__ (for UniMag Pro reader)
 - __shuttle__ (for Shuttle reader)


## Sample

var swiper_connected = false;
var swipe;
function onDeviceReady() {
   
    if (window.cordova) {

        cordova.plugins.unimag.swiper.activate();
        cordova.plugins.unimag.swiper.enableLogs(true);
        cordova.plugins.unimag.swiper.setReaderType('unimag_pro');

        swipe = function () {
            cordova.plugins.unimag.swiper.swipe(function successCallback() {
                alert('SUCCESS: Swipe started.');
            }, function errorCallback() {
                alert('ERROR: Could not start swipe.');
            });
        };

        cordova.plugins.unimag.swiper.on('connected', function () {
            swiper_connected = true;
        });

        cordova.plugins.unimag.swiper.on('disconnected', function () {
            swiper_connected = false;
        });

        cordova.plugins.unimag.swiper.on('swipe_success', function (e) {
            var data = JSON.parse(e.detail);
            alert('data: ' + JSON.stringify(data) );
        });

        cordova.plugins.unimag.swiper.on('swipe_error', function () {
            alert('swipe_error');
        });

        cordova.plugins.unimag.swiper.on('timeout', function (e) {
            if (swiper_connected) {
                alert('ERROR: Swipe timed out - ' + e.detail);
            } else {
                alert('ERROR: Connection timed out - ' + e.detail);
            }
        });
    }
}