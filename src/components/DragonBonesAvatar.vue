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

onMounted(() => {
  if (!canvas.value) {
    return;
  }

  // 1. Initialize PixiJS
  app = new PIXI.Application({
    view: canvas.value,
    width: 800,
    height: 600,
    backgroundColor: 0x1099bb,
    resizeTo: window,
  });

  // 2. Load DragonBones assets
  app.loader
    .add('skeleton', SKELETON_URL)
    .add('textureData', TEXTURE_DATA_URL)
    .add('texture', TEXTURE_URL)
    .load((loader, resources) => {
      // 3. Parse data and build armature
      const factory = PixiFactory.factory;
      factory.parseDragonBonesData(resources.skeleton.data, 'avatar');
      factory.parseTextureAtlasData(resources.textureData.data, resources.texture.texture, 'avatar');

      armatureDisplay = factory.buildArmatureDisplay('armature', 'avatar');
      if (!armatureDisplay) {
        console.error('Failed to build armature "armature"');
        return;
      }

      // 4. Add armature to stage and play animation
      app.stage.addChild(armatureDisplay);
      armatureDisplay.animation.play('wave');

      // Center the armature
      armatureDisplay.x = app.screen.width / 2;
      armatureDisplay.y = app.screen.height / 2;

      // 5. Add ticker for DragonBones WorldClock
      app.ticker.add((delta) => {
        // Pass the elapsed time in seconds to the DragonBones engine.
        PixiFactory.factory.dragonBones.advanceTime(delta / 60);
      });
    });
});

onUnmounted(() => {
  // 6. Clean up
  if (armatureDisplay) {
    armatureDisplay.destroy();
  }

  PixiFactory.factory.clear(true);

  if (app) {
    app.loader.reset();
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
