"use strict";

// Parent window
let parentWindow;
window.addEventListener("message", window_onMessage, false);

function window_onMessage(event) {
  parentWindow = event.source;

  switch (event.data.name) {
    case "setVolume": jklmAudio.setVolume(event.data.volume); break;
    case "joinGame": socket.emit("joinGame", event.data.gameId, event.data.roomCode, event.data.userToken); break;
  }
}

// Socket
const socket = io({ reconnection: false, transports: ["websocket"] });
socket.on("disconnect", socket_onDisconnect);

function socket_onDisconnect() {
  $hide(".page:not([hidden])");

  // Protect against race condition if chat socket is about to receive some redirect message
  setTimeout(() => {
    parentWindow.postMessage({ name: "disconnected" }, "*");
  }, 500);
}
