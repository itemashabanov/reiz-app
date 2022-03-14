<template>
  <button class="reiz-btn">{{ text }}</button>
</template>

<script>

export default {
  name: 'ReizBtn',

  props: {
    text: String,
  }
}
</script>

<style>
  .reiz-btn {
    border: 0;
    border-radius: 4px;
    cursor: pointer;
    padding: 6px 12px;
    background-color: aquamarine;
    font-size: 17px;
    transition: background-color .2s ease-in-out;
  }
  .reiz-btn:hover {
    background-color: paleturquoise;
  }
  .reiz-btn:not(:last-child) {
    margin-right: 14px;
  }
  .reiz-btn:last-child {
    margin-left: auto;
  }
  .reiz-btn--active {
    background-color: rgb(28, 138, 138);
    color: #fff;
  }
</style>
