"use strict";

const jklmAudio = (() => {
  const AudioContext = window.AudioContext != null ? window.AudioContext : window["webkitAudioContext"];
  const ctx = new AudioContext();

  const masterGainNode = ctx.createGain();
  masterGainNode.connect(ctx.destination);

  const suspendedElt = $make("div", document.body, { textContent: "🔇", hidden: true });
  suspendedElt.style.position = "absolute";
  suspendedElt.style.right = 0;
  suspendedElt.style.top = 0;
  suspendedElt.style.padding = "0.5em";
  suspendedElt.style.margin = "0.5em";
  suspendedElt.style.background = "#fff";
  suspendedElt.style.borderRadius = "2em";
  suspendedElt.style.opacity = 0.2;
  suspendedElt.hidden = ctx.state !== "suspended";

  function ensureAudioIsPlaying() {
    suspendedElt.hidden = ctx.state !== "suspended";
    if (ctx.state === "suspended") ctx.resume();
  }

  window.addEventListener("click", () => ensureAudioIsPlaying());
  window.addEventListener("keydown", () => ensureAudioIsPlaying());

  const audioBuffersByName = {};

  const load = (urlsByName) => {
    for (const name in urlsByName) {
      const url = urlsByName[name];

      fetch(url)
        .then((res) => res.arrayBuffer())
        .then((bytes) => ctx.decodeAudioData(bytes))
        .then((buffer) => audioBuffersByName[name] = buffer);
    }
  };

  const play = (name, options = {}) => {
    if (audioBuffersByName[name] == null) return;

    const source = ctx.createBufferSource();
    if (options.loop) source.loop = true;
    source.buffer = audioBuffersByName[name];
    source.connect(masterGainNode);

    // Don't queue sound if audio is suspended, unless it's looping
    if (ctx.state !== "suspended" || options.loop) source.start(0);

    return source;
  }

  const setVolume = (volume) => {
    suspendedElt.hidden = volume === 0 || ctx.state !== "suspended";
    masterGainNode.gain.value = volume;
  };

  return { load, play, setVolume };
})();
