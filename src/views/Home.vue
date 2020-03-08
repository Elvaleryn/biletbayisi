<template>
  <div>
    <Navbar />
    <FlightSearchBox />
  </div>
</template>

<script>
// @ is an alias to /src
import FlightSearchBox from "../components/FlightSearchBox";
import Navbar from "../components/Navbar";

export default {
  name: "Home",
  components: { FlightSearchBox, Navbar }
};
</script>

<style lang="scss" scoped>
.main-jumbotron {
  background-image: linear-gradient(
    to left,
    rgb(11, 65, 114),
    rgb(82, 153, 211)
  );
}
</style>
