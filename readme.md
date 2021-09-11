**** Installation /**
Clone this repository

git clone git@github.com:Abdou-Tzrt/realtime-single-page-forum-app.git

Go inside the directory

cd real-time-single-page-forum-app

***Now install composer

composer install
Generate key on .env file

cp .env.example .env
php artisan key:generate

**** Setting for Pusher ***
Go to .env file and change Broadcast drive to pusher

BROADCAST_DRIVER=pusher
and also give credentials of your pusher on .env file

PUSHER_APP_ID=xxxxxx
PUSHER_APP_KEY=xxxxxxxxxxxxxxxxxxxx
PUSHER_APP_SECRET=xxxxxxxxxxxxxxxxxxxxxx
PUSHER_APP_CLUSTER=xxx
open bootstrap.js file and provide pusher key there also.

window.Echo = new Echo({
    broadcaster: 'pusher',
    key: "xxxxxxxxxxxxxxxxxxxxx",
    cluster: "xxx",
    encrypted: true,
    ...

php artisan migrate
