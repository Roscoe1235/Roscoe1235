using Terraria;
using Terraria.ModLoader;
using Microsoft.Xna.Framework;

namespace YourModName.NPCs
{
    public class GreenTurtle : ModNPC
    {
        public override void SetStaticDefaults()
        {
        }

        public override void SetDefaults()
        {
            NPC.width = 76; // Width of a single frame in the sprite sheet
            NPC.height = 56; // Height of a single frame in the sprite sheet
            NPC.damage = 30;
            NPC.defense = 15;
            NPC.lifeMax = 300;
            NPC.HitSound = Terraria.ID.SoundID.NPCHit1;
            NPC.DeathSound = Terraria.ID.SoundID.NPCDeath2;
            NPC.value = 200f;
            NPC.knockBackResist = 0.1f;
            NPC.aiStyle = 39; // Turtle AI style

            // The following lines make the NPC hostile
            NPC.friendly = false;
            NPC.noTileCollide = false;
            NPC.noGravity = false;
            NPC.behindTiles = true;

            // Set the number of frames and frame height in the sprite sheet
            NPC.Framecount = 4; // Number of frames in the sprite sheet
            NPC.frameHeight = 56;
        }




        public override float SpawnChance(NPCSpawnInfo spawnInfo)
        {
            // Spawn on the surface of the jungle during the day
            return spawnInfo.Player.ZoneJungle && spawnInfo.SpawnTileY < Main.worldSurface && Main.dayTime ? 0.05f : 0f;
        }

        public override void AI()
        {
            Player player = Main.player[NPC.target];

            // If the player is close enough, make the GiantTortoise aggressive
            if (player != null && !player.dead)
            {
                float distance = NPC.Distance(player.Center);
                if (distance < 300)
                {
                    NPC.velocity = NPC.DirectionTo(player.Center) * 3; // Adjust the speed as needed
                    NPC.spriteDirection = NPC.velocity.X > 0 ? 1 : -1; // Set the sprite direction based on movement
                }
                else
                {
                    NPC.velocity = Vector2.Zero;
                }
            }
        }

        public override void FindFrame(int frameHeight)
        {
            // Determine the current frame based on the animation counter
            NPC.frameCounter++;
            if (NPC.frameCounter >= 10) // Adjust the frame switch speed as needed
            {
                NPC.frame.Y = (NPC.frame.Y + frameHeight) % (frameHeight * Npc.frameCount);
                NPC.frameCounter = 0;
            }
        }
    }
}
