<template>
  <canvas ref="canvas"></canvas>
</template>

<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue';
import * as PIXI from 'pixi.js';
import { PixiFactory } from 'dragonbones-pixijs';

const canvas = ref<HTMLCanvasElement | null>(null);
let app: PIXI.Application | null = null;
let armatureDisplay: any = null;

const SKELETON_URL = '/assets/avatar_ske.json';
const TEXTURE_DATA_URL = '/assets/avatar_tex.json';
const TEXTURE_URL = '/assets/avatar_tex.png';

onMounted(async () => {
  if (!canvas.value) {
    return;
  }

  // 1. Initialize PixiJS
  app = new PIXI.Application();
  await app.init({
    canvas: canvas.value,
    width: 800,
    height: 600,
    backgroundColor: 0x1099bb,
    resizeTo: window,
  });

  // 2. Load DragonBones assets
  await PIXI.Assets.load([SKELETON_URL, TEXTURE_DATA_URL, TEXTURE_URL]);

  // 3. Parse data and build armature
  const factory = PixiFactory.factory;
  const skeletonData = PIXI.Assets.get(SKELETON_URL);
  const textureData = PIXI.Assets.get(TEXTURE_DATA_URL);
  const texture = PIXI.Assets.get(TEXTURE_URL);

  factory.parseDragonBonesData(skeletonData);
  factory.parseTextureAtlasData(textureData, texture);

  armatureDisplay = factory.buildArmatureDisplay('armature');
  if (!armatureDisplay) {
    console.error('Failed to build armature');
    return;
  }

  // 4. Add armature to stage and play animation
  app.stage.addChild(armatureDisplay);
  armatureDisplay.animation.play('idle');

  // Center the armature
  armatureDisplay.x = app.screen.width / 2;
  armatureDisplay.y = app.screen.height / 2;

  // 5. Add ticker for DragonBones WorldClock
  app.ticker.add((ticker) => {
    // Pass the elapsed time in seconds to the DragonBones engine.
    PixiFactory.factory.dragonBones.advanceTime(ticker.deltaMS / 1000);
  });
});

onUnmounted(() => {
  // 6. Clean up
  if (armatureDisplay) {
    armatureDisplay.destroy();
  }

  PixiFactory.factory.clear(true);
  PIXI.Assets.unload([SKELETON_URL, TEXTURE_DATA_URL, TEXTURE_URL]);

  if (app) {
    app.destroy(true, true);
    app = null;
  }
});
</script>

<style scoped>
canvas {
  display: block;
}
</style>
