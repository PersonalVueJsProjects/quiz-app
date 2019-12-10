<template>
    <div>
        <div>
            <b-nav tabs>
                <b-nav-item disabled><b>fancy Quiz App</b></b-nav-item>
                <b-nav-item>{{numCorrect}}/{{numTotal}}</b-nav-item>
            </b-nav>
        </div>
    </div>
</template>


<script>
export default {
  props:[
      'numCorrect',
      'numTotal'
  ]  
}
</script>