# Singular Player SDK

Last updated: 2017-06-23

This document explains how to use the Singular.live Player SDK.

## Introduction

The Singular Player SDK provides Java Script libraries to create Singular Player instances, load compositions to the player, 
set payloads for loaded compositions, trigger animations, define and run sequences specified in a json format.

If you want to develop Singular Control Apps, Singular Widgets and Custom data services, please follow the links below:
 - [Singular Widget SDK](https://singularlive.zendesk.com/hc/en-us/articles/115003026388-Singular-Widget-SDK-Documentation)
 - [Singular App SDK](https://singularlive.zendesk.com/hc/en-us/articles/115005838428-Singular-APP-SDK)
 - [Singular REST API](https://singularlive.zendesk.com/hc/en-us/articles/115003104007--REST-API-Description)

## Table of Contents

1. **[Introduction](#introduction)**

1. **[Singular Player - Loading Compositions](#singular-player---loading-compositions)**
   * [Add the Singular Player SDK path into your code](#add-the-singular-player-sdk-path-into-your-code)
   * [Add iframe pointing to the player url](#add-iframe-pointing-to-the-player-url)
   * [Obtain player object from the iframe by the iframe id](#obtain-player-object-from-the-iframe-by-the-iframe-id)
   * [Load composition identified by the composition id](#load-composition-identified-by-the-composition-id)
   * [Code example](#code-example)

1. **[Singular Player - Handle SubCompositions](#singular-player---handle-subcompositions)**
   * [Get SubComposition by id](#get-subcomposition-by-id)
   * [Set Payload of a SubComposition](#set-payload-of-a-subcomposition)
   * [Code example](#code-example)

1. **[API Reference](#api-reference)**

1. **[Getting Help](#getting-help)**

## Singular Player - Loading Compositions

#### Add the Singular Player SDK path into your code.

```html
  <!-- Include the Singular Player JavaSrcipt library -->
  <script src="https://libs.singular.live/singularplayer/0.1.1/singularplayer.js"></script>
```

#### Add iframe pointing to the player url.

```html
  <iframe id="SingularPlayer" src="https://alpha.singular.live/singularplayer/client" 
          style='width:640px;height:320px;border:solid 1px black'>
  </iframe>
 ```

#### Obtain player object from the iframe by the iframe id.

```js
  // define Singular Player Instance
  var player;
  // create player object from iframe 'SingularPlayer'
  player = SingularPlayer("splayer");
```

#### Load composition identified by the composition id

```js
  // load composition with a specific id into the player
  // and output a status info 
  player.loadComposition(8702, function compositionLoaded() {
        $('#status').text('INFO: Composition loaded');
  });
```

#### Code example

This code snipplet defines an iFrame, creates a Singular Player object and loads a Singular composition.

Checkout the code example for [´Singular SDK - Load HTML5 Compositions´ on codepen.io](https://codepen.io/singular/pen/LLjxGK). 

```html
  <!-- load Singular Player into iFrame -->
  <iframe id="SingularPlayer" src="https://beta.singular.live/singularplayer/client"
          style='width:1280px; height:720px;border:solid 1px black'>
  </iframe>
  <!-- output status message here -->
  <b><div id='status'></div></b>

  <script>
      // define Singular Player Instance
      var player;

      $(document).ready(function () {

      // create player object from iframe 'splayer'
      player = SingularPlayer("SingularPlayer");

      // load composition with a specific id into the player 
      player.loadComposition(8702, function compositionLoaded() {
            $('#status').text('INFO: Composition loaded');
      });

      // Listen to error report
      player.addListener('error', function (event, msg) {
            if (event == 'error')
               $('#status').text("ERROR:" + msg.error.message);
            });
       });
  </script>
```

## Singular Player - Handle SubCompositions

One Composition can have one or many SubCompositions. A SubComposition basically is a Composition that's created or imported into a parent Composition. Like a Composition, a SubComposition is a group of Singular Widgets, it can access external data sources and link to real-time data. 

#### Get SubComposition by id

```js
  // define SubComposition Instance
  var subComposition;
  // create SubComposition object identified by the SubComposition name
  subComposition = player.getMainComposition().find(subCompName)[0];
```

#### Set Payload of a SubComposition

A SubComposition can expose parameters by defining `ControlNodes`. 
Following types of `ControlNodes` exist:

- `Text`: simple single line text, URLs,
- `TextArea`: multi line text
- `Number`: number
- `Image`: URL to an image
- `Color`: rgba values
- `Checkbox`: true, false
- `Gradient`: specify a rectangular or circular gradient
- `JSON Text`: text formatted as JSON
- `Button`: trigger functions

```js
  // set Payload of ControlNode "Location" of type "text" to "Munich, GER"
  // and ControlNode "Weather" of type "text" to "Sunny, 30C"
  subComposition.setPayload({
    Location: "Munich, GER",
    Weather: "Sunny, 30C"
  });

```

#### Code Example

This code snipplet creates a Singular Player object, loads a Singular composition and sets the payload of SubCompositions.

Checkout the code example for [´Singular SDK - Control and Trigger HTML5 Compositions´ on codepen.io](https://codepen.io/singular/pen/XgapYw). 

```html
  <script>
      // define Singular Player Instance
      var player;

      $(document).ready(function () {

      // create player object from iframe 'splayer'
      player = SingularPlayer("SingularPlayer");

      // load composition with a specific id into the player 
      player.loadComposition(8702, function compositionLoaded() {
            $('#status').text('INFO: Composition loaded');
      });

      // Listen to error report
      player.addListener('error', function (event, msg) {
            if (event == 'error')
               $('#status').text("ERROR:" + msg.error.message);
            });
       });

       function setText(id) {
         var subComp = player.getMainComposition().find(id)[0];
         if (id == 'subComp1') {
           var text = $('#Location').val();
           subComp.setPayload({
             Location: text
           });
         } else if (id == 'subComp2') {
           var text = $('#Weather').val();
           subComp.setPayload({
             Weather: text
           });
         }

         function showSubcomp(id, show) {
           var subComp = player.getMainComposition().find(id)[0];
          if (show || show === undefined)
            subComp.playTo('In');
          else
            subComp.playTo('Out');
          }
  </script>

  <div>
    <!-- load Singular Player into iFrame -->
    <iframe id="SingularPlayer" src="https://beta.singular.live/singularplayer/client"
            style='width:1280px; height:720px;border:solid 1px black'>
    </iframe>
    <!-- output status message here -->
    <b><div id='status'></div></b>
    
    <!-- add controls for sub-compositions -->
    <div style='position:relative; top:5px' id='control'>
        <div>subComp1 
            <input type='text' id='Location'>
            <button onclick='setText("subComp1")'>Set</button>
            <button onclick='showSubcomp("subComp1")'>Show</button>
            <button onclick='showSubcomp("subComp1", false)'>Hide</button>
        </div>
        <div>subComp2 
            <input type='text' id='Weather'>
            <button onclick='setText("subComp2")'>Set</button>
            <button onclick='showSubcomp("subComp2")'>Show</button>
            <button onclick='showSubcomp("subComp2", false)'>Hide</button>
        </div>
  </div>
```

## API Reference

- **`SingularPlayer`**: SingularPlayer Class
	+ `loadComposition(id, callback)`	load composition into player and execute callback
	+ `renderComposition(compJson, callback)`	render composition provided as json and execute callback
	+ `getMainComposition()`	get main composition loaded into the player
- **`Composition`**: Compositon Class
	+ `jumpTo(state)`	jump to defined animation state
	+ `playTo(state)`	run animation from current to defined state
	+ `seek(time)`	goto a specific time
	+ `setPayload(payload)`	set payload for the current composition
	+ `getSubcompositionById(id)`	get the subcomposition by the specified id. If the id is not defined, the main composition is returned
	+ `listSubcompositions()`	get an array of sub composition id:name 
[{id:name}]
	+ `getModel()`	get json structure of composition.
returns array of objects []
	+ `getPayload()`	get payload of composition.
returns array of objects []
	+ `getControlNode()`	returns object {}
	+ `getSequencer()`	get sequencer instance.
returns sequencer object
	sequencer	{start, stop, seek} all sequencers at once
- **`Sequencer`**: Sequencer Class	
	+ `start()`	start the selected sequencer
	+ `stop()`	stop the selected sequencer
	+ `seek()`	goto a specific time in the selected sequencer
	+ `setDuration()`	set the duration of the sequencer
	+ `setPayload()`	set sequencer payload

## Getting Help

- **Need help**? Ask a question to the [Singular Helpdesk](https://singularlive.zendesk.com/hc/en-us/requests/new).
- **Found a bug?** You can open a [GitHub issue](https://github.com/singularlive/singularplayer-samples/issues).

