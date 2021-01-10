<template>
  <div>
    <b-modal v-model="visible" title="RESULT" :ok-only="true" :hide-header-close="true" @ok="reload">
        <ul>
            <li>Correct {{numCorrect}}</li>
            <li>Answered {{numTotal}}</li>
            <li>Incorrect {{numTotal - numCorrect}}</li>
            <li>Unanswered {{10 - numTotal}}</li>
        </ul>
    </b-modal>
  </div>
</template>

<script>
export default {
    props :[
        'visible',
        'numTotal',
        'numCorrect'
    ],
    methods : {
        reload() {
            // a very bad way to reload
            location.reload();
        }
    }
    
}
</script>