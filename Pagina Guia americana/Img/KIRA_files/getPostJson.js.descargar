"use strict";

const headers = { "Content-Type": "application/json" };

function postJson(url, body, callback) {
  fetch(url, { method: "POST", headers, body: JSON.stringify(body) })
    .then((res) => {
      if (!res.ok) throw new Error(`httpStatusCode:${res.status}`);
      res.json().then((json) => callback(json));
    })
    .catch((error) => { callback({ errorCode: error.message }); });
}

function getJson(url, callback) {
  fetch(url, { method: "GET", headers })
    .then((res) => {
      if (!res.ok) throw new Error(`httpStatusCode:${res.status}`);
      if (res.status === 204) return callback(null);
      res.json().then((json) => callback(json));
    })
    .catch((error) => { callback({ errorCode: error.message }); });
}
