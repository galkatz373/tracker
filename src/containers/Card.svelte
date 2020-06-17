<script>
  import { numberWithCommas } from "./../globals.js";
  export let image, number, color, title;
</script>

<style lang="scss">
  .container {
    display: flex;
    margin: 2rem;
    flex-grow: 1;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    @media only screen and (max-width: 800px) {
      margin: 1rem 0.2rem;
      flex-basis: 100%;
    }

    img {
      height: 8rem;
      // margin: 1.5rem;

      @media only screen and (max-width: 800px) {
        height: 3rem;
      }
    }

    .title {
      text-align: center;
      font-weight: bold;
      font-size: 2rem;
      margin: 1rem 0 0.5rem 0;

      @media only screen and (max-width: 800px) {
        font-size: 1rem;
      }
    }

    .number {
      letter-spacing: 0.5mm;
      font-size: 2rem;
      padding: 0.2rem 1.5rem;
      border-radius: 5px;
      font-weight: bold;
      color: white;
      @media only screen and (max-width: 800px) {
        font-size: 1rem;
      }
    }
  }
</style>

<div class="container">
  <img alt="image" src={image} />
  <div class="title">{title}</div>
  <div class="number" style={`background-color:${color}`}>
    {numberWithCommas(number)}
  </div>
</div>
