<template>
  <div class="task3">
    <div class="kvadrat">
      <div class="kvadrat">
        <div class="kvadrat">
          <div class="kvadrat">
            <div class="kvadrat">
              <div class="kvadrat">
                <div class="kvadrat"><div class="kvadrat"></div></div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {

}
</script>

<style scoped>

.kvadrat {
  width: 100px;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid black;
}

.kvadrat > div {
  width: 85%;
  height: 85%;
  border: 1px solid black;
  display: flex;
  align-items: center;
  justify-content: center;
}

.task3 {
  margin-top: 20px;
 
}

</style>