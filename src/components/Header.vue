<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled><h4><b>Quizz</b></h4></b-nav-item>
      <b-nav-item disabled><h4>{{numCorrect}} / {{numTotal}}</h4></b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
    props: [
        'numCorrect',
        'numTotal'
    ]
}
</script>