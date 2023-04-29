---
showReadingTime: false
showTOC: false
title: Contact
showBreadCrumbs: false
disableShare: true
searchHidden: true
author: " "
---

{{< rawhtml >}}

<style>
  form{
    background: rgba(27,31,34,0.80);
    width: 640px;
    margin: 50px auto;
    max-width: 97%;
    border-radius: 4px;
    padding: 30px;
  }

  form .title h2{
    letter-spacing: 6px;
    display: inline-block;
    padding: 8px;
    margin-bottom: 32px;
    color: white;
  }

  form{
    border: 1px solid white;
  }

  form .half{
    display: flex;
    justify-content: space-between;
  }

  form .half .item{
    display: flex;
    flex-direction: column;
    margin-bottom: 24px;
    width: 48%;
    color: white;
  }
  
  .full{
      color: white;
  }

  form label{
    display: block;
    font-size: 13px;
    letter-spacing: 3.5px;
    margin-bottom: 16px;
  }

  form .half .item input{
    border-radius: 4px;
    border: 1px solid white;
    outline: 0;
    padding: 16px;
    width: 100%;
    height: 44px;
    background: transparent;
    font-size: 17px;
  }

  form .full{
    margin-bottom: 24px;
  }

  form .full textarea{
    background: transparent;
    border-radius: 4px;
    border: 1px solid white;
    outline: 0;
    padding: 12px 16px;
    width: 100%;
    height: 200px;
    font-size: 17px;
  }

  form .action{
    margin-bottom: 32px;
  }

  form .action input{
    background: transparent;
    border-radius: 4px;
    border: 1px solid white;
    cursor: pointer;
    font-size: 13px;
    font-weight: 600;
    height: 44px;
    letter-spacing: 3px;
    outline: 0;
    padding: 0 20px 0 22px;
    margin-right: 10px;
  }

  form .action input[type="submit"]{
    background: white;
    color: black;
  }

  form .half .item input:focus, form .full textarea:focus, form .action input[type="reset"]:hover{
    background: rgba(255,255,255,0.075);
  }

  form .action input[type="reset"]{
    color: white;
  }

  @media (max-width: 480px){
    form .half{
      flex-direction: column;
    }
    form .half .item{
      width: 100%;
    }
    form .action{
      display: flex;
      flex-direction: column;
    }
    form .action input{
      margin-bottom: 10px;
      width: 100%;
    }
  }

  .hidden{
      visibility: hidden;
  }
</style>

<div>
<form name="contact" method="POST" action="https://n8n.apps.codefusionz.com/webhook/send-mail">
    <div class="title">
      <h2>Let's Connect!</h2>
    </div>
    <div class="half">
      <div class="item">
        <label for="name">NAME</label>
        <input style="color:white;" type="text" id = "name" name="name" required>
      </div>
      <div class="item">
        <label for="email">EMAIL</label>
        <input style="color:white;" type="text" id = "email" name="email" required>
      </div>
    </div>
    <div class="full">
      <label for="message">MESSAGE</label>
      <textarea style="color:white;" name="" id = "message" name="message" required></textarea>
    </div>
    <div class="action">
      <input type="submit" value = "SEND MESSAGE">
      <input type="reset" value = "RESET">
    </div>
</form>
</div>

{{< /rawhtml >}}
