<template>
  <div>
    <div class='col-md-12 title'>Admin - Dostava</div>
    <wysiwyg class='p20' v-model="niz.text"/>
    <button type='button' class='btn btn-success' @click='deliver'>Potvrdi</button>
  </div>
</template>
<script>
export default{
  data: function() {
    return {
      niz : niz
  }
},
methods :{
  deliver:function(){
    $.ajax({
    url: window.base_url+'/delivery',
    type: 'PATCH',
    data: this.niz.text,
    dataType: 'json',
    success: function(data){
      
    },
    error: function(xhr, status, error){
      $('#err').html('Dogodila se greska - '+xhr.status+", poslat [id]: "+x).removeClass('nev');
    }
  });

},
ucitaj : function(){
  var self = this;
  $.ajax({
    url: window.base_url+'/delivery',
    type: 'GET',
    dataType: 'json',
    success: function(data){
      //ucitaj delivery
    },
    error: function(xhr, status, error){
      $('#err').html('Dogodila se greska - '+xhr.status).removeClass('nev');
    }
  });
},
beforeMount(){
  this.ucitaj()
}
}



}
var niz = {
  text: 'text'
}
</script>
