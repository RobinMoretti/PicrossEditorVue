<template>
	<div class="layer-editor">
		<h3>Tool</h3>

		<div class="input-group options-conainter">

			<div class="option">
				<ColorPickerToggler
					:color="activeColor"
					v-on:colorPicked="updateActiveColor($event)"></ColorPickerToggler>
			</div>

			<div class="option" v-if="activeLayer">
				<div 
					v-for="(color, key) in activeLayerColors"
					:key="'color-selectable-'+key"
					:style="{'background-color': color}"
					class="color-selectable"
					:class="{selected: activeColor.style == color}"
					v-on:click="pickedAvailableColor(color)"></div>
			</div>

		</div>
		
	</div>
</template>

<script>
import { mapGetters } from 'vuex'
import helpers from '../helpers'

export default {
	computed: {
		...mapGetters({
			activeTool: 'tool/activeTool',
			activeLayer: 'layer/activeLayer',
			activeLayerColors: 'layer/activeLayerColors'
		}),
		activeColor: function(){
			return this.$store.state.tool.activeColor
		}
	},
	methods: {
		pickedAvailableColor: function(color){
			var newColor = helpers.cloneVar(this.activeColor);
			newColor.style = color
			color =  color.match(/\d+/g);
			console.log(color)
			newColor.red = parseInt(color[0])
			newColor.green = parseInt(color[1])
			newColor.blue = parseInt(color[2])
			newColor.alpha = parseInt(color[3])
			this.updateActiveColor(newColor)
		},
		updateActiveColor: function(color){
			console.log('color', color)
			this.$store.commit('tool/updateActiveColor', { color: color })
		},
	},
	data () {
		return {
		}
	},
	props: {
	},
	mounted () {
	},
}
</script>

<style lang="scss" scoped>
	.layer-editor{
		font-family: 'pixel';
		box-sizing: border-box;
		font-size: calc(var(--cell-size)*4);
		line-height: calc(var(--cell-size)*2);
		border: solid var(--cell-size) black;
		padding: var(--cell-size);
		width: 100%;
		margin-bottom: var(--cell-size);

		.input-group,h3{
			margin-bottom: var(--cell-size);
			.sm{
				.input-container{
					input{	
					}
				}
				.input-value{
				}
			}
		}

		.input-group{
			display: flex;
			flex-direction: column;
			justify-content: flex-start;
			align-items: flex-start;
			padding-bottom: var(--cell-size);
			margin-bottom: var(--cell-size);
			border-bottom: black var(--cell-size) solid;
		}


		label{
			display: block;
			width: 100%;
			text-align: left;
		}

		.options-conainter{
			.option{
				width: 100%;
				display: flex;
				justify-content: space-between;
				align-items: center;
				margin-bottom: var(--cell-size);
				label{
					width: unset !important; 
				}

				.color-selectable{
					
					display: inline-block;
					width: calc(var(--cell-size) * 7);
					height: calc(var(--cell-size) * 4);

					border: solid black calc(var(--cell-size)/2);
					// border: solid black var(--cell-size);
					&.selected{
						border: solid black calc(var(--cell-size));
					}
				}
			}
		}

	}
</style>
