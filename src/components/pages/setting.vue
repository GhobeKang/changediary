<template>
  <f7-page>
    <f7-list>
      <f7-list-group>
        <f7-list-item>
          <div class="imageOverlay">
            <img id="profilePhoto" src="../../assets/img/best-hd-unknown-clipart-pictures.jpg" class="img-circle img-responsive center-block" @click="changeProfile">
            <img id="overlayImg" src="../../assets/img/profile_change.png">
          </div>
        </f7-list-item>
        <f7-list-item>
          <span id="userId_set"><strong>{{userId}}</strong></span>
        </f7-list-item>
      </f7-list-group>
    </f7-list>
  </f7-page>
</template>

<script>
  import firebase from '../../assets/js/firebaseInit'
  import 'vue-croppa/dist/vue-croppa.css'

  export default {
    data: function () {
      return {
        userId: window.userId
      }
    },
    methods: {
      changeProfile: function () {
        var preloader = window.f7.mainView.router.loadPage('/popup/photoEdit/')
        if (!preloader) window.f7.mainView.router.load({url: '/popup/photoEdit/'})
      }
    },
    created: function () {
      var $$ = window.Dom7
      var firebaseRef = firebase.initNode.database().ref('/users')
      var auth = firebase.initNode.auth()
      firebaseRef.orderByKey().on('value', function (snap) {
        snap.forEach(function (data) {
          if (data.val().email === auth.currentUser.email) {
            if (data.hasChild('profilePhoto')) {
              $$('#profilePhoto').attr('src', data.val().profilePhoto)
            }
          }
        })
      })
    }
  }
</script>
