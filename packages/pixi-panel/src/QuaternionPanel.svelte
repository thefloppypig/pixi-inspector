<script lang="ts">
    import NumberField from "blender-elements/src/NumberField/NumberField.svelte";
    import Property from "blender-elements/src/Property/Property.svelte";
    import { Quaternion } from "three";
    import { createEventDispatcher } from "svelte";

    export let x : number | undefined
    export let y : number | undefined
    export let z : number | undefined
    export let w : number | undefined
    
    const quaternion = new Quaternion()
    if (x !== undefined && y !== undefined && z !== undefined && w !== undefined) {
        quaternion.set(x,y,z,w);
    }
    const dispatch = createEventDispatcher();
    
    function updateQuaternion() {
        if (x !== undefined && y !== undefined && z !== undefined && w !== undefined) {
            quaternion.set(x,y,z,w);
        }
        return dispatch("change", quaternion)
    }
</script>

<div class="quaternionPanel">
    <Property label="Quaternion X" hint="The rotation of the 3D object in quaternion representation" group>
        <NumberField
        value={x}
        step={0.1}
        min={-1}
        max={1}
        location="TOP"
        suffix=""
        on:change={(e) => {
            x = e.detail
            updateQuaternion()
        }}
        />
    </Property>
    <Property label="Y" group>
        <NumberField
        value={y}
        step={0.1}
        min={-1}
        max={1}
        location="MIDDLE"
        suffix=""
        on:change={(e) => {
            y = e.detail
            updateQuaternion()
        }}        
        />
    </Property>
    <Property label="Z" group>
        <NumberField
        value={z}
        step={0.1}
        min={-1}
        max={1}
        location="MIDDLE"
        suffix=""
        on:change={(e) => {
            z = e.detail
            updateQuaternion()
        }}        
        />
    </Property>
    <Property label="W" group>
        <NumberField
        value={w}
        step={0.1}
        min={-1}
        max={1}
        location="BOTTOM"
        suffix=""
        on:change={(e) => {
            w = e.detail
            updateQuaternion()
        }}        
        />
    </Property>
</div>