---
layout: default
permalink: /BlockChainz/APIDocumentation
---

# Hexa-Game REST API Documentation


## Board API


### Create

- description: creates a board
- request: `POST /api/board/create/`
    - content-type: `application/json`
    - body: object
      - name: (string) the name of the board
      - size: (int) the size of the board
      - nullspaces: (list) the locations of nullspaces on the board
- response: 200
    - content-type: `application/json`
    - body: object
      - name: (string) the name of the board
      - size: (int) the size of the board
      - nullspaces: (list) the locations of nullspaces on the board
      - redTotal: (int) the total number of ruby pieces on the board
      - blueTotal: (int) the total number of pearl pieces on the board
      - currPlayer: (string) the player who's turn it is (ruby/pearl)
- response: 500
    - body: Internal server error
``` 
$ curl -H "Content-Type: application/json" -X POST -d 
'{"name":"game1","size":50, "nullspaces":[(0,0,0)]}' 
localhost:3000/api/board/create/
```

- description: Checks if a player has any moves left
- request: `POST /api/board/options/`
    - content-type: `application/json`
    - body: object
      - hexagons: (list) the list of all hexagons on the board
      - state: (string) the player to be checked for options
- response: 200
    - content-type: `application/json`
    - body: boolean
      - noMoves: (boolean) true if the user has no moves left, false otherwise
- response: 400
    - body: hexagons/state is missing
``` 
$ curl -H "Content-Type: application/json" -X POST -d 
'{"hexagons":[{_id: 5ac51ac7c2e6bd0e88d5718d, props: { className: 'ruby' }, q: -2, r: -2, s: 4, board: '5ac51ac7c2e6bd0e88d57181', __v: 0 }, { _id: 5ac51ac7c2e6bd0e88d57194, props: { className: 'empty' }, q: -1, r: -3, s: 4, board: '5ac51ac7c2e6bd0e88d57181', __v: 0 }],"state":"ruby"}' 
localhost:3000/api/board/options/
```

### Read

- description: Gets all the board objects
- request: `GET /api/board/all/`
- response: 200
    - content-type: `application/json`
    - body: object
      - boards: (list of board objects)
        - board: (object)
          - name: (string) the name of the board
          - size: (int) the size of the board
          - nullspaces: (list) the locations of nullspaces on the board
          - redTotal: (int) the total number of ruby pieces on the board
          - blueTotal: (int) the total number of pearl pieces on the board
          - currPlayer: (string) the player who's turn it is (ruby/pearl)
- response: 500
    - body: Internal server error
``` 
$ curl localhost:3000/api/board/all/
```

- description: Gets the board object given the id of the board
- request: `GET /api/board/:id/`
- response: 200
    - content-type: `application/json`
    - body: object
      - board: (object)
        - name: (string) the name of the board
        - size: (int) the size of the board
        - nullspaces: (list) the locations of nullspaces on the board
        - redTotal: (int) the total number of ruby pieces on the board
        - blueTotal: (int) the total number of pearl pieces on the board
        - currPlayer: (string) the player who's turn it is (ruby/pearl)
- response: 500
    - body: Internal server error
``` 
$ curl localhost:3000/api/board/5ac51ac7c2e6bd0e88d57181/

```

- description: Gets the board given the id of a player on that board
- request: `GET /api/user/:id/board/`
- response: 200
    - content-type: `application/json`
    - body: object
      - boards: (list of board objects)
        - board: (object)
          - name: (string) the name of the board
          - size: (int) the size of the board
          - nullspaces: (list) the locations of nullspaces on the board
          - redTotal: (int) the total number of ruby pieces on the board
          - blueTotal: (int) the total number of pearl pieces on the board
          - currPlayer: (string) the player who's turn it is (ruby/pearl)
- response: 500
    - body: Internal server error
- response: 404
    - body: Board not found, no current game in session
``` 
$ curl localhost:3000/api/user/5ac51ac7c2e6bd0e88d5718d/board/all/
```


### Update

- description: Updates the database when the game is finished
- request: `PATCH /api/board/:id/gamedone/`
- response: 200
    - content-type: `application/json`
    - body: object
      - board: (object)
          - name: (string) the name of the board
          - size: (int) the size of the board
          - nullspaces: (list) the locations of nullspaces on the board
          - redTotal: (int) the total number of ruby pieces on the board
          - blueTotal: (int) the total number of pearl pieces on the board
          - currPlayer: (string) the player who's turn it is (ruby/pearl)
- response: 500
    - body: Internal Server error
- response: 404
    - body: Board not found, no current game in session

``` 
$ curl -X PATCH localhost:3000/api/board/5ac51ac7c2e6bd0e88d57181/gamedone/
```

## User API

### Create

app.post('/api/user/new', function(req, res, next) {
    const newUser = req.body;
    User.save(newUser, function(err, user) {
      if (err) return res.status(500).end('Internal Server error');
      res.json(user);
    });
  });

- description: create a new user
- request: `POST /api/user/new/`
    - content-type: `application/json`
    - body: object
      - googleId: (string) the google id, stored safely
- response: 200
    - content-type: `application/json`
    - body: object
      - _id: (string) the user id
      - googleId: (string) the google id of the user
- response: 500
    - body: Internal Server error
``` 
$ curl -X POST 
       -H "Content-Type: `application/json`" 
       -d '{"googleId":"123@gmail.com"}
       http://localhost:3000/api/user/new/
```

## Hexagon API


### Update

- description: preforms the clone functionality, 1 position away
- request: `PATCH /api/hexagon/:id/clone/`
    - content-type: `application/json`
    - body: object
      - q: (int) the q position of the hexagon
      - r: (int) the r position of the hexagon
      - s: (int) the s position of the hexagon
- response: 200
    - content-type: `application/json`
    - body: list of objects
      - Hexagons:
        - _id: (string) the Hexagon id
        - q: (int) the q position of the hexagon
        - r: (int) the r position of the hexagon
        - s: (int) the s position of the hexagon
- response: 403
    - body: Invalid Move
- response: 500
    - body: Internal Server error
``` 
$ curl -H "Content-Type: application/json" -X PATCH -d 
'{"q": -2, "r": -2, "s": 4}' 
localhost:3000/api/hexagon/:id/clone/
``` 
- description: preforms the jump functionality, 2 position away
- request: `PATCH /api/hexagon/:id/jump/`
    - content-type: `application/json`
    - body: object
      - q: (int) the q position of the hexagon
      - r: (int) the r position of the hexagon
      - s: (int) the s position of the hexagon
- response: 200
    - content-type: `application/json`
    - body: list of objects
      - Hexagons:
        - _id: (string) the Hexagon id
        - q: (int) the q position of the hexagon
        - r: (int) the r position of the hexagon
        - s: (int) the s position of the hexagon
- response: 403
    - body: Invalid Move
- response: 500
    - body: Internal Server error
``` 
$ curl -H "Content-Type: application/json" -X PATCH -d 
'{"q": -2, "r": -2, "s": 4}'
localhost:3000/api/hexagon/:id/jump/
```
