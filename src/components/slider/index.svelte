<script lang="ts">
  export let minValue: number;
  export let maxValue: number;
  export let defaultValue: number = 0;
  export let afterChange: (val: number) => void;
  export let title
  export let step = 1
  export let value = undefined;


  const id = Math.random().toString().substring(0, 6)

  const onChange = (e) => {
    value = e.target.value;
    afterChange(value)
  }


</script>

<div class="slider">
  <span>{title}</span>
  <input type="range" id={id} min={minValue} max={maxValue} value={value || defaultValue} step={step} on:input={onChange}/>
  <label for={id}>{value || defaultValue}</label>
</div>

<style>
  .slider {
    display: inline-block;
    white-space: nowrap;
  }
  .slider span {
    vertical-align: middle;
    font-size: 16px;
    display: inline-block;
    min-width: 52px;
  }
  .slider input { 
    margin-bottom: 0;
    vertical-align: middle;
  }
  .slider label {
    display: inline-block;
    margin-right: 24px;    
  }
</style>