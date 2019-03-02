<template>
	<div>
		<div class="bg" v-bind:style="bgStyle">

		</div>
		<border v-bind:window="windows['game']">
				test
		</border>
		<border v-bind:window="windows['tracker']">
				tracker
		</border>
	</div>
</template>

<script lang="ts">
import Border from './border/border.vue'
const bgImage = require('./bg.jpg');
const clockRep = nodecg.Replicant('clock');

class Pt {
	x: number;
	y: number;

	constructor(x: number, y: number) {
		this.x = x;
		this.y = y;
	}
}

class Rect {
	p1: Pt;
	p2: Pt;

	constructor(x1: number, y1: number, x2: number, y2: number) {
		this.p1 = new Pt(x1,y1);
		this.p2 = new Pt(x2,y2);
	}
}

class Window {
	rect: Rect;
	cutout: boolean;
	constructor(x1: number, y1: number, x2: number, y2: number, cutout: boolean) {
		this.cutout = cutout;
		this.rect = new Rect(x1, y1, x2, y2);
	}
}

export default {
	data() {
		const windows = {
			"game": new Window(665, 60, 1860, 1020, true),
			"tracker": new Window(10, 10, 100, 100, false),
		};
		const cutouts = Object.entries(windows).reduce( (acc, i): Rect[] => {
			const w: Window = i[1];
			if (w.cutout) {
				acc.push(w.rect);
			}

			return acc;

		}, []);
		return {
			windows: windows,
			bgStyle: {
				backgroundImage: `url(${bgImage})`,
				clipPath: calcClipPath(cutouts)
			}

		};
	},
	created() {
	},
	methods: {
	},
	components:{
		"border": Border
	}
};

function calcClipPath(rects: Rect[]): string {
	let path = 'polygon(0px 0px';

	for (const rect of rects) {
		path += `, ${rect.p1.x}px 0px`;
		path += `, ${rect.p1.x}px ${rect.p1.y}px`;
		path += `, ${rect.p1.x}px ${rect.p2.y}px`;
		path += `, ${rect.p2.x}px ${rect.p2.y}px`;
		path += `, ${rect.p2.x}px ${rect.p1.y}px`;
		path += `, ${rect.p1.x}px ${rect.p1.y}px`;
		path += `, ${rect.p1.x}px 0px`;
	}

	path += ', 1920px 0px, 1920px 1080px, 0px 1080px, 0px 0px)';

	console.log(path);
	return path;
}
</script>

<style lang="scss">
  .bg {
	  position: absolute;
	  left: 0px;
	  top: 0px;
	  width: 1920px;
	  height: 1080px;
   }
</style>