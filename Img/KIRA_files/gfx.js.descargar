"use strict";

const jklmGfx = (() => {
  const load = (url) => {
    const image = new Image();
    image.src = url;
    return image;
  };

  const draw = (ctx, image, x, y) => {
    x = x | 0;
    y = y | 0;

    ctx.drawImage(image, x - image.width / 2, y - image.height / 2);
  };

  return { load, draw };
})();
