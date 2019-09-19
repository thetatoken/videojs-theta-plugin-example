# Video.js Plugin for Theta

Video.js player plugin for Theta. Currently only supports HLS.js compatible video.

## Setup

To integrate Theta into your video.js powered player, simply include the theta scripts and adjust the options for your video.js player.

### Include scripts

Enter the following script tags in your ```head``` tag.


#### HLS.js

Ensure HLS.js is included before the theta-hls-plugin.

```html
<script src="js/theta.js"></script>
<script src="js/theta-hls-plugin.js"></script>
<script src="js/videojs-theta-plugin.js"></script>
```

### Theta Wallet token generator

To allow authenticated users to earn TFUEL in your application, your backend needs to generate a Theta Wallet access token. Please implement a function which will call your backend to generate a new access token for the user's Theta Wallet.

```javascript
async function getWalletAccessToken() {
    //Check if a user is logged in...
    let isAuthenticated = true;

    if (!isAuthenticated) {
        //No user is logged in, no wallet will be used
        return null;
    }

    //This API should check the user's auth 
    let body = await yourAPIRequestToGenerateThetaWalletAccessTokenForAuthedUser();

    //Return the access token from the request body
    return body.access_token;
}
```

Note: If you do not have a secret key to generate a Theta Wallet access token, please contact your Theta rep for access.


### Setup video.js options


```html
<script>
  const player = window.player = videojs('my-player', {
          techOrder: ["theta_hlsjs", "html5"],
          sources: [{
            src: "YOUR_VIDEO_URL",
            type: "application/vnd.apple.mpegurl",
            label: "1080p"
          }],
          theta_hlsjs: {
              videoId: "YOUR_INTERNAL_VIDEO_ID",
              userId: "YOUR_AUTHED_USER_ID",
              walletUrl: "wss://api-wallet-service.thetatoken.org/theta/ws",
              onWalletAccessToken: getWalletAccessToken,
              hlsOpts: hlsOpts
          }
      });
</script>
```

## License

Copyright (c) Theta Labs, Inc.
