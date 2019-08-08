# Running Spotnik Locally

1) Download and install [Docker Desktop](https://hub.docker.com/).
2) Download the [docker-compose.yaml, Spotnik-Backend and Spotnik-Frontend](https://drive.google.com/open?id=1D7nBAQry2kMSwdGLxTV27EZoYd3NNPEh) files, and put all of them together in the same directory.
3) Download [ngrok](https://ngrok.com/) in order to allow outside connections to your computer. Update the ngrok configuration (located at `$HOME/.ngrok2/ngrok.yml`) by adding the following configuration:
```
tunnels:
	spotnik-backend:
	  addr: 8080
	  proto: http
	spotnik-frontend:
	  addr: 80
	  proto: http
```
4) Run `ngrok start --all`, and write down the tunnels links redirecting to localhost:80 and localhost:8080 (e.g. `https://9ac2269d.ngrok.io`). The tunnel redirecting to localhost:80 will allow other users to load the frontend from their devices (give them that address!), and the tunnel redirecting to localhost:8080 will be used by Spotify. 
5) Create a [Spotify integration](https://developer.spotify.com/dashboard/login), and update its redirect uri on by appending `/authorize` to the address of the tunnel redirecting to localhost:8080. It should look like this: `https://f430ef88.ngrok.io/authorize`. Take this opportunity to write down the Client Id and Client Secret of your integration.
6) Update the docker-compose.yaml file environment variables:
	- Update `SPOTIFY_APP_ID` and `SPOTIFY_APP_SECRET` with the Client Id and the Client Secret from the previous step.
  - Update `REDIRECT_HOST` with the address of the tunnel redirecting to `localhost:8080` (don't add `/authorize` here).
  - Update `ADMIN_USERNAME` and `ADMIN_PASSWORD` to whatever you'd like - just remember what you put in there.
7) Open a terminal where the spotnik-backend.tar and spotnik-frontend.tar files are located, and run `docker load --input spotnik-backend.tar` and `docker load --input spotnik-frontend.tar`.
8) Run `docker-compose up`. Spotnik is starting! 
9) Wait for Spotnik to finish loading (it will be ready for use when you see `Unable to send GetActiveTrack request, no access token` in the logs).
10) Go to `http://localhost:8080/admin/login` in your browser. It will prompt for the admin credentials you configured earlier, and redirect you to Spotify in order to log in with your account.
11) Let Spotify know which device you want to use by playing any random song for a few seconds (make sure that you are logged with your account).
12) That's it, you're all set! Open your browser, and go load the frontend by going to the address of the tunnel redirecting to localhost:80. 

The following admin endpoints are available:
- `http://localhost:8080/admin/login` in order to log in with your Spotify account.
- `http://localhost:8080/admin/logout` in order to log out.
- `http://localhost:8080/api/admin/queue/next` to admin skip a song.