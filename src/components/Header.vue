<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled><b>Кариерен советник</b></b-nav-item>
      <b-nav-item disabled
        >Прашање: {{ numCorrect + 1 }}/{{ numTotal }}</b-nav-item
      >
    </b-nav>
  </div>
</template>

<script>
export default {
  props: ["numCorrect", "numTotal"],
};
</script>
