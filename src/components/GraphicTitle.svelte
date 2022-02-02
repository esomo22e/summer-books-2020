<script>

   export let title;
   export let subhed;

</script>

<style>
   span.graphictitle {
      /* font-family:'akkurat','Helvetica','Arial',sans-serif; */
      /* font-family: 'Nunito', sans-serif; */
      font-family: 'Montserrat', sans-serif;

      font-size:3rem;
      font-weight:900;
      display:block;
      text-align:center ;
      margin: 20px;
      color: #4b3a82;
      width: 30%;
margin: 40px auto 0;
border-bottom: 12px solid #F0F8FF;

/* border-bottom: 20px double #F0F8FF; */
   }

   @media screen and (max-width:600px) {
       span.graphictitle{
           width: 50%;
          font-size:2.2rem;

           margin: 20px auto 10px;

       }
}
</style>

<span class="graphictitle">{title}</span>
<!-- <span class="graphicsubhed">{subhed}</span> -->
