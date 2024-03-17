const { Client, Intents } = require('discord.js');
const client = new Client({ intents: [Intents.FLAGS.GUILDS, Intents.FLAGS.GUILD_MEMBERS] });

client.on('ready', () => {
  console.log(`Logged in as ${client.user.tag}!`);
  setInterval(assignRoles, 604800000); // 604800000 milliseconds = 1 week
});

function assignRoles() {
  const guild = client.guilds.cache.get('929458173016940604'); // Replace with your guild ID
  guild.members.cache.forEach(member => {
    // Get roles that start with 'staff' and sort them by position
    const staffRoles = member.roles.cache
      .filter(role => role.name.startsWith('staff'))
      .sort((a, b) => b.position - a.position); // Sort roles by position in descending order

    if (staffRoles.size > 0) {
      const highestStaffRole = staffRoles.first();
      // Find the next higher role
      const higherRole = guild.roles.cache
        .filter(role => role.position > highestStaffRole.position)
        .sort((a, b) => a.position - b.position) // Sort roles by position in ascending order
        .first();

      if (higherRole) {
        // Add the next higher role to the member
        member.roles.add(higherRole).catch(console.error);
      }
    }
  });
}

client.login('MTIxODg0NTg0NjE1MDQ1MTI2MA.G17XL6.QovOBlraS_aNXSxYot46JVLAWkt_4zA505qBpA'); // Replace with your bot token
