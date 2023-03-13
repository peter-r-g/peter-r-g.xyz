<template>
  <BaseArea title="My Skills">
    <ul>
      <li>C#</li>
      <li>Lua</li>
      <li>Java</li>
      <li>JavaScript</li>
      <li>HTML/(S)CSS</li>
      <li>Git</li>
      <li>Unity</li>
      <li>Docker</li>
      <li>PostgreSQL</li>
      <li>Visual Studio</li>
      <li>Visual Studio Code</li>
      <li>JetBrains Rider</li>
      <li>Windows</li>
      <li>Linux</li>
    </ul>

    <BaseCard>
      <table>
        <tr>
          <td>
            <h4>Game Development</h4>
            Experienced in writing C# code in game engines and desktop applications.
          </td>
        </tr>
        <tr>
          <td>
            <h4>Server-side development using Node.js</h4>
            Skilled in creating backends for web applications
          </td>
        </tr>
      </table>
    </BaseCard>

  </BaseArea>
</template>

<style lang="scss" scoped>
@import "~assets/variables.scss";

table {
  border-collapse: collapse;
  width: 100%;

  tr:nth-child(even) {
    background-color: $dark-700;
  }

  tr:nth-child(odd) {
    background-color: $dark-800;
  }

  td,
  th {
    padding: 10px;
  }
}

ul {
  list-style: none;
  padding: 0;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;

  li {
    margin: 5px;
    padding: 10px 20px;
    background-color: $dark-800;
    border-radius: 5px;
    border: 1px solid rgba($dark-600, 0.2);
    text-align: center;
  }
}
</style>
