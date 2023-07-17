"use strict";

let wereSettingsMigrated = false;

const settings = (() => {
  const text = localStorage.getItem("jklmSettings");

  const defaultSettings = {
    version: 2,
    volume: 0.5,
    muted: false,
    chatFilter: [],
  };

  let tempSettings = text != null ? JSON.parse(text) : defaultSettings;

  switch (tempSettings.version) {
    case 1:
      tempSettings.volume = 0.5;
      tempSettings.muted = false;

      // (Voluntary fallthrough)
      tempSettings.version = 2;

    case 2:
      // Current version
      break;

    default:
      console.log(`Found unsupported settings version ${tempSettings.version}, clearing.`);

      tempSettings = defaultSettings;
      wereSettingsMigrated = true;
      break;
  }

  if (tempSettings.nickname == null) {
    tempSettings.nickname = "Guest" + (1000 + Math.floor(Math.random() * 9000)).toString();
    wereSettingsMigrated = true;
  }

  if (tempSettings.chatFilter == null) {
    tempSettings.chatFilter = [];
    wereSettingsMigrated = true;
  }

  return tempSettings;
})();

if (wereSettingsMigrated) saveSettings();

function saveSettings() {
  if (settings.nickname.length > 20) settings.nickname = settings.nickname.substring(0, 20);

  localStorage.setItem("jklmSettings", JSON.stringify(settings));
}

function hasGuestNickname() {
  return /^Guest[0-9]{4}$/.test(settings.nickname);
}

function getAuthServiceName(service) {
  switch (service) {
    case "discord": return "Discord";
    case "twitch": return "Twitch";
    case "jklm": return "JKLM.FUN";
    default: return service;
  }
}

// Generate a persistent user token for reconnection during a game
function getUserToken() {
  const existingToken = localStorage.getItem("jklmUserToken");
  if (existingToken != null && existingToken.length === 16) return existingToken;

  const array = new Uint8Array(16);
  crypto.getRandomValues(array);
  let token = "";
  const digits = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+-";
  for (let i = 0; i < array.length; i++) token += digits[array[i] % digits.length];
  localStorage.setItem("jklmUserToken", token);
  return token;
}
