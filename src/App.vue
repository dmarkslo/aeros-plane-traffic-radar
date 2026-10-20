
<template>
  <h1>You did it!</h1>
  <p>
    Visit <a href="https://vuejs.org/" target="_blank" rel="noopener">vuejs.org</a> to read the
    documentation
  </p>
</template>

<script setup lang="ts">


  getFlightsData()
  


  type FlightData = {
    time: number
    states: PlaneState[] | null
  }

  type PlaneState = [
      icao24: string,                 // 0
      callsign: string | null,        // 1
      originCountry: string,          // 2
      timePosition: number | null,    // 3
      lastContact: number,            // 4
      longitude: number | null,       // 5
      latitude: number | null,        // 6
      baroAltitude: number | null,    // 7
      onGround: boolean,              // 8
      velocity: number | null,        // 9
      trueTrack: number | null,       // 10
      verticalRate: number | null,    // 11
      sensors: number[] | null,       // 12
      geoAltitude: number | null,     // 13
      squawk: string | null,          // 14
      spi: boolean,                  // 15
      positionSource: number,        // 16
      category?: number              // 17 — optional extended field
  ]

  
  function metersPerSecondToKmh(velocity: number): number {
    return velocity * 3.6
  }
  function logFlightData(data: FlightData){

    if (data.states === null){
      console.log("No Aircraft found!")
      return
    }

    let unix_timestamp = data.time
    var date = new Date(unix_timestamp * 1000)
    console.log("Time: %s:%s", date.getHours(), date.getMinutes())
    for (const plane of data.states){
      if (plane[1] != ""){ //Only show flights with callsigns
        console.log("Callsign: %s Origin: %s Squawk: %s Velocity: %f km/h", plane[1],plane[2],plane[14],metersPerSecondToKmh(plane[9]))
      }
    }

  }

  async function getFlightsData(){

    // GET podatki o letalih nad slovenijo (aprox. boundary)
    const url = '/api/states/all?lamin=45.42&lomin=13.37&lamax=46.88&lomax=16.61'
    
    try {
      const response = await fetch(url)
      if (!response.ok){
        throw new Error(`Response status:' %{response.status}`)
      }

      const data = await response.json()
      logFlightData(data)

    }  catch (error){
      console.error(error)
    }
  }

</script>
<style scoped></style>
