# Document of **Jorney-to-the-west** source code 

**Yang Daming 2018-9**



[TOC]

:shield: :shinto_shrine: :monkey_face: :video_game: :pig_nose::mountain::crossed_swords: 



## Modules function :



### :deciduous_tree: Map 

Our script files for map module are [Map.js](src/MyGame/Map/Map.js) and [MapParser.js](src/MyGame/Map/MapParser.js) 

The main functionality of [Map.js](src/MyGame/Map/Map.js) is :

1. Read in JSON files describing the current map

The most inmportant functionalities of [MapParser.js](src/MyGame/Map/MapParser.js) are :

1. Parse the data in the JSON files
2. Render the map and NPC in that map
3. Detect events on certain map
4. Judge whether a pile in the map is walkable



Now, I am trying to explain how we make our map 2.5D via one of the JSON file affliated to each map.

The JSON file is called `*-dat.json` (\* represents for the map, like `wanggong-dat.json`), and it looks like : 

```json
{
  "width": 19,
  "height": 19,
  "data": [
    200, 200, 200, 200, 100, 200, 100, 200, 100, ... ...
  ],
  "content": {
    "10*": "walkable",
    "11*": "wevent",
    "20*": "unwalkable",
    "21*": "uevent",
    "*": "height"
  },
  "born": [9.5, 14.5]
}

```

The basic geometrical information of a map are all included in this file, like 'width', 'height', indicating 				  			  			 	this is a 19 piles by 19 piles map. And there are 19 * 19 values in the data array, corresponding to 19 * 19 	piles. The meanings of each value are written in key 'content'. To be more specific :

The hundreds digit represents for whether a pile is walkable or not, so when we parse the file and decide if our hero can walk on a certain pile, this digit can tell us the answer.

The tens digit indicates if this pile will trigger an event, both walkable and un-walkable piles can be  triggers for events.

And the units digit stands for the height of the pile. With this digit we can prevent hero from walking straight to a higher place from the ground (since both of the piles are walkable). We set certains rules for this digit:

1.  Piles of same units digit are connected
2.  Piles of ajacent units digit, like 1 and 2, or 2 and 3, are also connected
3.  Other scenarios are all divided.

So, we can set the units digits of ground piles with 1, stairs with 2, mountains with 3.

Finally, the key 'born' is the birth place of hero.



Another approach to make our map 2.5D is using the [Engine_LayerManager.js](src/Engine/Core/Engine_LayerManager.js) to draw a multi-layer map, like :

```javascript
    gEngine.LayerManager.addToLayer(gEngine.eLayer.eBackground, this.mMapBkg);

    var i;
    for (i = 0; i < this.mMyNPC.length; ++i)
       gEngine.LayerManager.addToLayer(gEngine.eLayer.eActors,this.mMyNPC[i].getNPC());

    gEngine.LayerManager.addToLayer(gEngine.eLayer.eActors, this.mMyHero.getHero());

    gEngine.LayerManager.addToLayer(gEngine.eLayer.eFront, this.mMapFrg);
```

And a sample foreground is image is like below (the background of this image is transparent) :

![foreground](./assets/map/wanggong/wanggong-frg.png)A sample Background is like below :

![background](./assets/map/wanggong/wanggong-bkg.png)

With this method, our hero can hide behind a pillar when he is at the back of the pillar, and can block it when he is at the front of it. 



### :monkey_face: Hero



###  :bulb: Event



### :video_game: System



