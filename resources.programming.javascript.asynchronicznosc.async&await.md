---
id: AfMNOapcyRX6yh6jKglI7
title: Async&Await
desc: ''
updated: 1631868846328
created: 1631188694093
---
Obsługa błędów w async await
```javascript
async function foo() {
    try {
        const image = await promiseFunction();
    } catch(err) {
        err => console.log(err);
    }
}
```

```javascript
import axios from "axios";

console.clear();

const fetchUsers = () =>
  axios.get("https://jsonplaceholder.typicode.com/users/");

const fetchAlbums = () =>
  axios.get(`https://jsonplaceholder.typicode.com/albums`);

const fetchPhotos = () =>
  axios.get(`https://jsonplaceholder.typicode.com/photos`);

const albumFactory = (albums, userId, photos) =>
  albums
    .filter((album) => album.userId === userId)
    .map((album) => ({
      ...album,
      photos: photos.filter((photo) => photo.albumId === album.id)
    }));

const userFactory = (user, albums, photos) => ({
  ...user,
  albums: albumFactory(albums, user.id, photos)
});

async function fetchUsersSetup() {
  const { data: users } = await fetchUsers();
  const { data: albums } = await fetchAlbums();
  const { data: photos } = await fetchPhotos();

  const usersWithAlbumsMapper = ({ users, albums, photos }) =>
    users.map((user) => userFactory(user, albums, photos));

  return usersWithAlbumsMapper({ users, albums, photos });
}

const setup = async () => {
  try {
    const result = await fetchUsersSetup();
    console.log(result);
  } catch (e) {
    console.log(e);
  } finally {
    console.log("Finished");
  }
};

setup();

```