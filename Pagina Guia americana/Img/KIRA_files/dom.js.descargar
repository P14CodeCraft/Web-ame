"use strict";

document.addEventListener("keydown", (event) => {
  // Prevent accidental back navigations in Firefox (use Alt+Left instead!)
  if (event.code === "Backspace") {
    if ((event.target.tagName === "INPUT" || event.target.tagName === "TEXTAREA") && !event.target.disabled && !event.target.readOnly) return;

    event.preventDefault();
  }
});

function isLargeScreen() { return matchMedia("(min-width: 1000px)").matches; }

const $ = (first, second) => {
  if (typeof first === "string") return document.querySelector(first);
  else if (second != null) return first.querySelector(second);
  return first;
};

const $$ = (first, second) => {
  if (typeof first === "string") return document.querySelectorAll(first);
  else return first.querySelectorAll(second);
};

function $make(tagName, parent, props) {
  const elt = document.createElement(tagName);
  if (parent != null) parent.appendChild(elt);
  for (const key in props) {
    if (key === "dataset") {
      for (const dataKey in props[key]) elt.dataset[dataKey] = props.dataset[dataKey];
    } else elt[key] = props[key];
  }
  return elt;
}

function $makeText(text, parent) {
  const node = document.createTextNode(text);
  if (parent != null) parent.appendChild(node);
  return node;
}

function $show(elt, visible) { $(elt).hidden = visible != null ? !visible : false; }
function $hide(elt) { $(elt).hidden = true; }

// Trigger a layout to help with transitions
function $layout(elt) { $(elt).offsetLeft; }

// Range inputs
function $bindRangeField(field, changeCallback) {
  field.number.addEventListener("input", (event) => field.range.value = field.number.value);
  field.range.addEventListener("input", (event) => field.number.value = field.range.value);

  field.number.addEventListener("change", (event) => changeCallback());
  field.range.addEventListener("change", (event) => changeCallback());
}

function $setRangeField(field, options) {
  if (options.min) field.number.min = field.range.min = options.min;
  if (options.max) field.number.max = field.range.max = options.max;
  if (options.step) field.number.step = field.range.step = options.step;
  field.number.value = field.range.value = options.value;
}

const tooltipsByElt = new WeakMap();

function $setTooltip(elt, text) {
  const hadTooltip = elt.dataset["tooltipText"] != null;
  elt.dataset["tooltipText"] = text;

  if (!hadTooltip) {
    let tooltipDiv = null;

    elt.addEventListener("mouseenter", (event) => {
      tooltipDiv = $make("div", document.body);
      tooltipsByElt.set(elt, tooltipDiv);

      tooltipDiv.style.pointerEvents = "none";
      tooltipDiv.style.position = "absolute";
      tooltipDiv.style.opacity = "0.7";
      tooltipDiv.style.fontSize = "smaller";

      const rect = elt.getBoundingClientRect();
      tooltipDiv.style.left = `${rect.x}px`;
      tooltipDiv.style.right = `calc(100vw - ${rect.x + rect.width}px)`;
      tooltipDiv.style.top = `calc(${Math.round(rect.y + rect.height)}px - 0.25em)`;
      tooltipDiv.style.display = "flex";
      tooltipDiv.style.flexFlow = "column";
      tooltipDiv.style.alignItems = "center";
      tooltipDiv.style.justifyContent = "center";

      const arrow = $make("div", tooltipDiv);
      arrow.style.width = "0";
      arrow.style.height = "0"
      arrow.style.borderLeft = "0.5em solid transparent";
      arrow.style.borderRight = "0.5em solid transparent";
      arrow.style.borderBottom = "0.5em solid black";

      const text = $make("div", tooltipDiv, { className: "text", textContent: elt.dataset["tooltipText"] });
      text.style.whiteSpace = "nowrap";
      text.style.background = "#000";
      text.style.color = "#fff";
      text.style.borderRadius = "2em";
      text.style.padding = "0.5em 0.75em";
      text.style.textAlign = "center";
    });

    elt.addEventListener("mouseleave", (event) => {
      if (tooltipDiv != null) {
        tooltipDiv.parentElement.removeChild(tooltipDiv);
        tooltipDiv = null;
        tooltipsByElt.delete(elt);
      }
    });
  } else {
    const tooltipDiv = tooltipsByElt.get(elt);
    if (tooltipDiv != null) $(tooltipDiv, ".text").textContent = text;
  }
}