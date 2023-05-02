<script lang="ts">
    import NumberField from "blender-elements/src/NumberField/NumberField.svelte";
    import Property from "blender-elements/src/Property/Property.svelte";
    import Toggle from "blender-elements/src/Toggle/Toggle.svelte";
    import { Euler, Quaternion } from "three";
    import { createEventDispatcher } from "svelte";
    import { exclude_internal_props } from "svelte/internal";

    enum EditRotataion3dMode {
        EditQuaternion = 0,
        EditEuler = 1,
    }

    export let x : number | undefined
    export let y : number | undefined
    export let z : number | undefined
    export let w : number | undefined
    
    let mode = 0

    const deg2rad = Math.PI/180;
    const rad2deg = 180/Math.PI;

    const quaternion = new Quaternion()
    const euler =  new Euler();

    const dispatch = createEventDispatcher();

    let eulerx : number 
    let eulery : number 
    let eulerz : number

    $: {
        quaternion.set(x??0,y??0,z??0,w??0);
        euler.setFromQuaternion(quaternion);
        destructure()
    }
    $: {
        euler.set(eulerx * deg2rad,eulery * deg2rad,eulerz * deg2rad);
        quaternion.setFromEuler(euler); 
        destructure()
    }

    function destructure() {
        eulerx = euler.x * rad2deg;
        eulery = euler.y * rad2deg;
        eulerz = euler.z * rad2deg;
        ({x,y,z,w} = quaternion);
    }

    function updateQuaternion() {
        if (x !== undefined && y !== undefined && z !== undefined && w !== undefined) {
            quaternion.set(x,y,z,w);
            euler.setFromQuaternion(quaternion);
            destructure()
        }
        return dispatch("change", quaternion)
    }

    function updateQuaternionEuler() {
        euler.set(eulerx * deg2rad,eulery * deg2rad,eulerz * deg2rad);
        quaternion.setFromEuler(euler); 
        destructure()
        return dispatch("change", quaternion)
    }
</script>

<div class="quaternionPanel">
    <Property label="Edit Mode">
        <div class="three-columns">
            <Toggle
                label="Quaternion"
                value={mode === EditRotataion3dMode.EditQuaternion}
                location="LEFT"
                on:change={() => {mode = EditRotataion3dMode.EditQuaternion}}
                
            />
            <Toggle
                label="Euler"
                value={mode === EditRotataion3dMode.EditEuler}
                location="RIGHT"
                on:change={() => {mode = EditRotataion3dMode.EditEuler}}
                
            />
        </div>
    </Property>
    {#if mode === EditRotataion3dMode.EditQuaternion}
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
    {/if}
    {#if mode === EditRotataion3dMode.EditEuler}
    <Property label="Euler X" hint="The rotation of the 3D object in euler angle representation" group>
        <NumberField
        value={eulerx}
        step={1}
        location="TOP"
        suffix=""
        on:change={(e) => {
            eulerx = e.detail
            updateQuaternionEuler()
        }}
        />
    </Property>
    <Property label="Y" group>
        <NumberField
        value={eulery}
        step={1}
        location="MIDDLE"
        suffix=""
        on:change={(e) => {
            eulery = e.detail
            updateQuaternionEuler()
        }}        
        />
    </Property>
    <Property label="Z" group>
        <NumberField
        value={eulerz}
        step={1}
        location="BOTTOM"
        suffix=""
        on:change={(e) => {
            eulerz = e.detail
            updateQuaternionEuler()
        }}        
        />
    </Property>
    {/if}
</div>