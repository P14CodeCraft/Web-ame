"use strict";

let textEntries = {};

const dateTimeFormat = new Intl.DateTimeFormat();

function loadText(path, language, callback) {
  getJson(`${path}/${language}.json`, (res) => {
    if (res == null) {
      if (language.includes("-")) loadText(path, language.split("-")[0], callback);
      else finish();
      return;
    }

    textEntries = res;

    for (const elt of $$("*[data-text]")) {
      if (textEntries[elt.dataset.text] != null) elt.textContent = textEntries[elt.dataset.text];
    }

    for (const elt of $$("*[data-html]")) {
      if (textEntries[elt.dataset.html] != null) elt.innerHTML = textEntries[elt.dataset.html];
    }

    for (const elt of $$("*[data-title-text]")) {
      if (textEntries[elt.dataset.titleText] != null) elt.title = textEntries[elt.dataset.titleText];
    }

    for (const elt of $$("*[data-placeholder-text]")) {
      if (textEntries[elt.dataset.placeholderText] != null) elt.placeholder = textEntries[elt.dataset.placeholderText];
    }

    finish();
  });

  function finish() {
    for (const elt of $$("*[data-date]")) {
      const dateText = elt.dataset.date;
      const year = parseInt(dateText.substring(0, 4), 10);
      const month = parseInt(dateText.substring(5, 7), 10);
      const day = parseInt(dateText.substring(8, 10), 10);
      elt.textContent = dateTimeFormat.format(Date.UTC(year, month - 1, day));
    }

    document.documentElement.lang = language;
    callback();
  }
}

function getText(fallback, id, data) {
  let text = textEntries[id];
  if (text == null) text = fallback;

  if (data != null) {
    for (const [key, value] of Object.entries(data)) text = text.replace(`{${key}}`, value);
  }

  return text;
}