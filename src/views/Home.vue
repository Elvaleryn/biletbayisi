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
