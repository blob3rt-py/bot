# bot
import discord
import json
import random
from discord.ext import commands, tasks
from itertools import cycle
import asyncio


bot = commands.Bot(command_prefix='$')

TOKEN = 'ODkzMTkzOTkyODkwNjE3OTM3.YVX5ug.Nwvb0jg8Xq7RYeSm63he9PBiX00'
status = cycle(['Yassin is sexy af', 'blobBOT', 'blob'])


@bot.event #when the bot logs on to its account
async def on_ready():
    await bot.change_presence(status=discord.Status.online)
    change_status.start()
    print('Logged in as {0.user}'.format(bot))

@tasks.loop(seconds=30) #cycles in between these 4 statuses every 10secs
async def change_status():
    await bot.change_presence(activity= discord.Game(next(status)))

def is_it_me(ctx):
    return ctx.author.id == 512279518073978881

@bot.command(alises = ['speach', 'speech'])
async def speech(ctx):
    embed = discord.Embed(title="Hello!", description= "I am blobBot created by @blobert\nI am still in the works so some commands might not work.\nI won't be online 24/7 \nTo see my commands type: $commands\nTo use my commands use the '$' prefix\nIf you have any suggestions/complaints dm me",colour=discord.Colour.green())
    embed.set_footer(text=f'{ctx.author} ', icon_url=ctx.author.avatar_url)
    await ctx.send(embed=embed)


@bot.event #memebr joining server
async def joined(member, ctx):
    embed = discord.Embed(title="Welcome", description=f"Hello, {member.mention} you look like shit ",
                          colour=discord.Colour.blue())
    await ctx.send(embed=embed)

@bot.event#member leaving server mesage
async def on_member_leave(member, ctx):
    embed = discord.Embed(title="FUCK YOU", description=f"Goodbye, {member.mention} no one likes you anyway ",
                          colour=discord.Colour.blue())
    await ctx.send(embed=embed)

@bot.command() #kick user
#@commands.has_any_role(842386587514175548)
async def kick(ctx, member: discord.Member, *, reason =None):
    await member.kick(reason = reason)
    embed = discord.Embed(title="KICKED", description=f"{member.mention} has been kicked, lmaoo loser\n Reason {reason}",
                          colour=discord.Colour.red())
    embed_sendl = await ctx.send(embed=embed)
    await embed_sendl.add_reaction('🇨')
    await embed_sendl.add_reaction('🇾')
    await embed_sendl.add_reaction('🇦')


@bot.command() #ban user
#@commands.has_any_role(842386587514175548,894213237816569897)
async def ban(ctx, member: discord.Member, *, reason =None):
    await member.ban(reason = reason)
    embed = discord.Embed(title="BANNED", description=f"{member.mention} has been banned, lmaoo cya never\n Reason: {reason}",
                          colour=discord.Colour.red())
    embed_send = await ctx.send(embed=embed)
    await embed_send.add_reaction('🇨')
    await embed_send.add_reaction('🇾')
    await embed_send.add_reaction('🇦')
    await embed_send.add_reaction('🇱')
    await embed_send.add_reaction('🇴')
    await embed_send.add_reaction('🇸')
    await embed_send.add_reaction('🇪')
    await embed_send.add_reaction('🇷')


@bot.command() #unban user
#@commands.has_any_role(842386587514175548,894213237816569897)
async def unban(ctx,*, member):
    banned_users = await ctx.guild.bans()
    member_name, memberdiscriminator = member.split('#')
    for ban_entry in banned_users:
        user = ban_entry.user
        if (user.name,user.discriminator) == (member_name, memberdiscriminator):
            await ctx.guild.unban(user)
            embed = discord.Embed(title="UNBANNED",
                                  description=f"{member.mention} has been unbanned, welcome back bitch",
                                  colour=discord.Colour.blue())
            embed_send44 = await ctx.send(embed=embed)
            await embed_send44.add_reaction('😭')


@bot.command(aliases = ['8ball'])
async def _8ball(ctx, *, question):
    responses = ["As I see it, yes.", "Ask again later.", "Better not tell you now.", "Cannot predict now.",
                 "Concentrate and ask again.", "Don’t count on it.", "It is certain.", "It is decidedly so.",
                 "Most likely.", "My reply is no.", "My sources say no.", "Outlook not so good.", "Outlook good.",
                 "Reply hazy, try again.", "Signs point to yes.", "Very doubtful.", "Without a doubt.", "Yes.",
                 "Yes – definitely.", "You may rely on it."]
    embed = discord.Embed(title="8ball says...", description=f'Question: {question}\nAnswer: {random.choice(responses)}',colour=discord.Colour.purple())
    await ctx.send(embed=embed)

@bot.command(alises= ['h8ball', 'hball'])
async def h8ball(ctx):
    embed_for_h8ball = discord.Embed(title="$8ball command help",description="to activate this command type:\n $ball (question)\n\nEXAMPLE:\n$8ball am i cool?",colour=discord.Colour.gold())
    await ctx.send(embed=embed_for_h8ball)


@bot.command() #deletes an amount of messages from a channel
@commands.has_permissions(manage_messages=True)
#@commands.has_any_role(842386587514175548)
async def clear(ctx, amount: int): #how many messages we want to delete # 5 is default value
    await ctx.channel.purge(limit = amount)
    return

@bot.command()
async def ping(ctx):
    embed = discord.Embed(title="Pong", description=f"{round(bot.latency * 1000)} ms",colour=discord.Colour.gold())
    await ctx.send(embed=embed)


@bot.command()
#@commands.has_any_role(842386587514175548,894213237816569897)
@commands.has_permissions(manage_messages=True)
async def mute(ctx, member: discord.Member, *, reason=None):
    guild = ctx.guild
    mutedRole = discord.utils.get(guild.roles, name="Muted")
    if not mutedRole:
        mutedRole = await guild.create_role(name="Muted")

        for channel in guild.channels:
            await channel.set_permissions(mutedRole, speak=False, send_messages=False, read_message_history=True, read_messages=True)
    embed = discord.Embed(title='MUTED', description=f"{member.mention} was muted, lmaoo loser", colour=discord.Colour.red())
    embed.add_field(name="reason:", value=reason, inline=False)
    embed_send =  await ctx.send(embed=embed)
    await member.add_roles(mutedRole, reason=reason)
    await embed_send.add_reaction('🇱')
    await embed_send.add_reaction('🇲')
    await embed_send.add_reaction('🇦')
    await embed_send.add_reaction('🇴')

@bot.command()
#@commands.has_any_role(842386587514175548,894213237816569897)
@commands.has_permissions(manage_messages=True)
async def unmute(ctx, member: discord.Member):
   mutedRole = discord.utils.get(ctx.guild.roles, name="Muted")
   await member.remove_roles(mutedRole)
   embed = discord.Embed(title="UNMUTED", description=f"{member.mention} was unmuted, i'm watching you...",colour=discord.Colour.green())
   embed_send33 = await ctx.send(embed=embed)
   await embed_send33.add_reaction('😭')


@clear.error #handles errors with specific task
async def clear_error(ctx, error):
    if isinstance(error, commands.MissingRequiredArgument):
        await ctx.send('Specify the amount of messages to delete')
        return


@bot.command(aliases= ['commands', 'command'])
async def _commands(ctx):
    pages = 4
    cur_page = 1
    embed1 = discord.Embed(title="MOD COMMANDS⤵",description=f'\n$poll (creates a poll)\n$hpoll (helps set up $poll)\n$role (reaction roles)\n$hrole (helps set up $role)$clear (clears a nr. of messages)\n$spam (spams a nr. of messages)\n$hspam (helps with $spam)\n$mute (mutes a user)\n$unmute (un-mutes a user)\n$kick (kicks a user)\n$ban (bans a user)\n$unban (un-bans a user)\n\nPage: 1/{pages}\n◀  previous page\n▶  next page',colour=discord.Colour.gold())
    embed2 = discord.Embed(title="FUN COMMANDS⤵",description=f"\n\n$8ball (answers a question)\n $fact (fun fact)\n\nPage: 2/{pages}\n◀  previous page\n▶  next page",colour=discord.Colour.gold())
    embed3 = discord.Embed(title="GAME COMMANDS⤵",description=f"\nSTILL IN PROGRESS\n\nPage: 3/{pages}\n◀  previous page\n▶  next page",colour=discord.Colour.gold())
    embed4 = discord.Embed(title="OTHER COMMANDS⤵",description=f"\n$ping (pings the bot)\n\nPage: 4/{pages}\n◀  previous page\n▶  next page",colour=discord.Colour.gold())
    embed1.set_footer(text=f'{ctx.author} ', icon_url=ctx.author.avatar_url)
    embed2.set_footer(text=f'{ctx.author} ', icon_url=ctx.author.avatar_url)
    embed3.set_footer(text=f'{ctx.author} ', icon_url=ctx.author.avatar_url)
    embed4.set_footer(text=f'{ctx.author} ', icon_url=ctx.author.avatar_url)

    message = await ctx.send(embed= embed1)
    contents = [embed1, embed2, embed3, embed4]
    # getting the message object for editing and reacting

    await message.add_reaction("◀️")
    await message.add_reaction("▶️")

    def check(reaction, user):
        return user == ctx.author and str(reaction.emoji) in ["◀️", "▶️"]
        # This makes sure nobody except the command sender can interact with the "menu"

    while True:
        try:
            reaction, user = await bot.wait_for("reaction_add", timeout=60, check=check)
            # waiting for a reaction to be added - times out after x seconds, 60 in this
            # example

            if str(reaction.emoji) == "▶️" and cur_page == 1:
                cur_page += 1
                await message.edit(embed= embed2)
                await message.remove_reaction(reaction, user)
            elif str(reaction.emoji) == "▶️" and cur_page == 2:
                cur_page += 1
                await message.edit(embed= embed3)
                await message.remove_reaction(reaction, user)
            elif str(reaction.emoji) == "▶️" and cur_page == 3:
                cur_page += 1
                await message.edit(embed= embed4)
                await message.remove_reaction(reaction, user)



            elif str(reaction.emoji) == "◀️" and cur_page == 4:
                cur_page -= 1
                await message.edit(embed= embed3)
                await message.remove_reaction(reaction, user)
            elif str(reaction.emoji) == "◀️" and cur_page ==3:
                cur_page -= 1
                await message.edit(embed= embed2)
                await message.remove_reaction(reaction, user)
            elif str(reaction.emoji) == "◀️" and cur_page ==2:
                cur_page -= 1
                await message.edit(embed= embed1)
                await message.remove_reaction(reaction, user)




            else:
                await message.remove_reaction(reaction, user)
                # removes reactions if the user tries to go forward on the last page or
                # backwards on the first page
        except asyncio.TimeoutError:
            await message.delete()
            break
            # ending the loop if user doesn't react after x seconds
@bot.event
async def on_message(message):
    if message.content.startswith("$fact"):
        lines = open('facts').read().splitlines()
        myline = random.choice(lines)
        await message.channel.send(myline)
    else:
        await bot.process_commands(message)




@bot.event #tell the user they cant use mod level commands
async def on_command_error(ctx, error):
    error = getattr(error, 'original', error)
    if isinstance(error, commands.MissingPermissions):
        embed = discord.Embed(title="YOU CANNOT USE THIS COMMAND",description=f"Are you mad fam, you are not a mod",colour=discord.Colour.red())
        embed_send01 = await ctx.send(embed=embed)
        await embed_send01.add_reaction('🇾')
        await embed_send01.add_reaction('🇴')
        await embed_send01.add_reaction('🇺')
        await embed_send01.add_reaction('🇹')
        await embed_send01.add_reaction('🇷')
        await embed_send01.add_reaction('🇮')
        await embed_send01.add_reaction('🇪')
        await embed_send01.add_reaction('🇩')
    elif isinstance(error, commands.MissingRequiredArgument):
        embed = discord.Embed(title="Error", description="Fill in all required fields\n\nSee $commands for help ",colour=discord.Colour.red())
        embed_send22 = await ctx.send(embed=embed)
        await embed_send22.add_reaction('🇱')
        await embed_send22.add_reaction('🇴')
        await embed_send22.add_reaction('🇸')
        await embed_send22.add_reaction('🇪')
        await embed_send22.add_reaction('🇷')



numbers = ("1️⃣", "2⃣", "3⃣", "4⃣", "5⃣",
		   "6⃣", "7⃣", "8⃣", "9⃣", "🔟")

@bot.command() #makes a  poll
async def poll(ctx, question=None, *options):
    if len(options) > 10:
        ten = discord.Embed(title='ERROR', description="You can only put a maximum of 10 options\n\n Type $hpoll to see help", colour=discord.Colour.red())
        embed_send1 = await ctx.send(embed=ten)
        await embed_send1.add_reaction('🇱')
        await embed_send1.add_reaction('🇴')
        await embed_send1.add_reaction('🇸')
        await embed_send1.add_reaction('🇪')
        await embed_send1.add_reaction('🇷')
    elif question == None:
        alert_embed = discord.Embed(title='ERROR', description="Write a question\n\n Type $hpoll to see help", colour=discord.Colour.red())
        embed_send2 = await ctx.send(embed = alert_embed)
        await embed_send2.add_reaction('🇱')
        await embed_send2.add_reaction('🇴')
        await embed_send2.add_reaction('🇸')
        await embed_send2.add_reaction('🇪')
        await embed_send2.add_reaction('🇷')
        return
    else:
        embed = discord.Embed(title= "⠀⠀⠀NEW POLL⠀⠀⠀" ,description=f"⠀{question}", colour=discord.Colour.blue())
        fields = [("Options:", "\n".join([f"{numbers[idx]}⠀{option}" for idx, option in enumerate(options)]), False)]
        for name, value, inline in fields:
            embed.add_field(name=name,value=value, inline=inline)
            embed.set_footer(text=f'{ctx.author} ', icon_url=ctx.author.avatar_url)
        msg_send = await ctx.send(embed=embed)
        for emoji in numbers[:len(options)]:
            await msg_send.add_reaction(emoji)

@bot.command()
async def hpoll(ctx):
    pages = 2
    cur_page = 1
    poll_embed = discord.Embed(title="How to use $poll command",description="1. Type $poll\n2. Write your question inside parenthesis\n3. Write your answers\n\nPage: 1/3\n▶  next\n◀  previous",colour=discord.Colour.gold())
    poll_embed.set_footer(text=f'{ctx.author} ', icon_url=ctx.author.avatar_url)
    embed2 = discord.Embed(title="Example command", description="\n $poll ''Are you cool?'' Yes no maybe\n\nPage: 2/3",colour=discord.Colour.gold())
    embed2.set_footer(text=f'{ctx.author} ', icon_url=ctx.author.avatar_url)
    poll_embed.set_footer(text=f'{ctx.author} ', icon_url=ctx.author.avatar_url)
    embed2.set_footer(text=f'{ctx.author} ', icon_url=ctx.author.avatar_url)

    embed = discord.Embed(title="Output:\n⠀⠀⠀NEW POLL⠀⠀⠀", description="⠀⠀⠀Are you cool?", colour=discord.Colour.blue())
    embed.add_field(name="Options:", value='\n1️⃣ yes\n2⃣  no\n3⃣  maybe', inline= False)
    embed.set_footer(text=f'{ctx.author} ', icon_url=ctx.author.avatar_url)



    contents = [poll_embed, embed2]
    pages = 2
    cur_page = 1
    message = await ctx.send(embed= poll_embed)
    # getting the message object for editing and reacting

    await message.add_reaction("◀️")
    await message.add_reaction("▶️")

    def check(reaction, user):
        return user == ctx.author and str(reaction.emoji) in ["◀️", "▶️"]
        # This makes sure nobody except the command sender can interact with the "menu"

    while True:
        try:
            reaction, user = await bot.wait_for("reaction_add", timeout=120, check=check)
            # waiting for a reaction to be added - times out after x seconds, 60 in this
            # example

            if str(reaction.emoji) == "▶️" and cur_page == 1:
                cur_page += 1
                await message.edit(embed = embed2)
                await message.remove_reaction(reaction, user)
            elif str(reaction.emoji) == "▶️" and cur_page == 2:
                cur_page += 1
                await message.edit(embed = embed)
                await message.remove_reaction(reaction, user)

            elif str(reaction.emoji) == "◀️" and cur_page == 2:
                cur_page -= 1
                await message.edit(embed = poll_embed)
                await message.remove_reaction(reaction, user)
            elif str(reaction.emoji) == "◀️" and cur_page == 3:
                cur_page -= 1
                await message.edit(embed = embed2)
                await message.remove_reaction(reaction, user)

            else:
                await message.remove_reaction(reaction, user)
                # removes reactions if the user tries to go forward on the last page or
                # backwards on the first page
        except asyncio.TimeoutError:
            await message.delete()
            break
            # ending the loop if user doesn't react after x seconds

@bot.command()
async def role(ctx):
    await ctx.send("Answer these questions")
    questions = ["Enter Message:", "Enter Emojis:", "Enter Roles:", "Enter Channel: "]
    answers = []
    def check(user):
        return user.author == ctx.author and user.channel == ctx.channel
    for question in questions:
        await ctx.send(question)
        try:
            msg = await bot.wait_for('message', timeout=340, check=check)
        except asyncio.TimeoutError:
            await ctx.send('Time out, you took too long')
            return
        else:
            answers.append(msg.content)

    emojis = answers[1].split(" ")
    roles = answers[2].split(" ")
    c_id = int(answers[3][2:-1])
    channel = bot.get_channel(c_id)

    bot_msg = await channel.send(answers[0])

    with open("selfrole.json", "r") as f:
        self_roles = json.load(f)

    self_roles[str(bot_msg.id)] = {}
    self_roles[str(bot_msg.id)]["emojis"] = emojis
    self_roles[str(bot_msg.id)]["roles"] = roles

    with open("selfrole.json", "w") as f:
        json.dump(self_roles, f)

    for emoji in emojis:
        await bot_msg.add_reaction(emoji)


@bot.event
async def on_raw_reaction_add(payload):
    msg_id = payload.message_id

    with open("selfrole.json", "r") as f:
        self_roles = json.load(f)

    if payload.member.bot:
        return

    if str(msg_id) in self_roles:
        emojis = []
        roles = []

        for emoji in self_roles[str(msg_id)]['emojis']:
            emojis.append(emoji)

        for role in self_roles[str(msg_id)]['roles']:
            roles.append(role)

        guild = bot.get_guild(payload.guild_id)

        for i in range(len(emojis)):
            choosed_emoji = str(payload.emoji)
            if choosed_emoji == emojis[i]:
                selected_role = roles[i]

                role = discord.utils.get(guild.roles, name=selected_role)

                await payload.member.add_roles(role)



@bot.event
async def on_raw_reaction_remove(payload):
    msg_id = payload.message_id

    with open("selfrole.json", "r") as f:
        self_roles = json.load(f)

    if str(msg_id) in self_roles:
        emojis = []
        roles = []

        for emoji in self_roles[str(msg_id)]['emojis']:
            emojis.append(
                emoji)

        for role in self_roles[str(msg_id)]['roles']:
            roles.append(role)

        guild = bot.get_guild(payload.guild_id)

        for i in range(len(emojis)):
            choosed_emoji = str(payload.emoji)
            if choosed_emoji == emojis[i]:
                selected_role = roles[i]

                role = discord.utils.get(guild.roles, name=selected_role)

                member = await(guild.fetch_member(payload.user_id))
                if member is not None:
                    await member.remove_roles(role)


@bot.command()
async def hrole(ctx):
    pages = 2
    cur_page = 1
    poll_embed = discord.Embed(title="How to use $role command",description="1. Type $roll\n2. Enter your message\n3. Enter the emojis\n4. Enter the roles\n5. Enter the channel\n\nPage: 1/3\n◀  previous page\n▶  next page",colour=discord.Colour.gold())
    poll_embed.set_footer(text=f'{ctx.author} ', icon_url=ctx.author.avatar_url)
    embed2 = discord.Embed(title="Example command", description="\n1. $roles\n\n 2. Get your roles here:\n🟦 = @test1\n🟥 = @test2\n\n3. 🟦 🟥\n\n4. test test1\n\n5. #test\n\nPage: 2/3\n◀  previous page\n▶  next page",colour=discord.Colour.gold())
    embed2.set_footer(text=f'{ctx.author} ', icon_url=ctx.author.avatar_url)
    poll_embed.set_footer(text=f'{ctx.author} ', icon_url=ctx.author.avatar_url)
    embed2.set_footer(text=f'{ctx.author} ', icon_url=ctx.author.avatar_url)

    embed = discord.Embed(title="Output:\nGet your roles here:", description="🟦 = test1\n🟥 = test2", colour=discord.Colour.blue())
    embed.set_footer(text=f'{ctx.author} ', icon_url=ctx.author.avatar_url)



    contents = [poll_embed, embed2]
    pages = 2
    cur_page = 1
    message = await ctx.send(embed= poll_embed)
    # getting the message object for editing and reacting

    await message.add_reaction("◀️")
    await message.add_reaction("▶️")

    def check(reaction, user):
        return user == ctx.author and str(reaction.emoji) in ["◀️", "▶️"]
        # This makes sure nobody except the command sender can interact with the "menu"

    while True:
        try:
            reaction, user = await bot.wait_for("reaction_add", timeout=120, check=check)
            # waiting for a reaction to be added - times out after x seconds, 60 in this
            # example

            if str(reaction.emoji) == "▶️" and cur_page == 1:
                cur_page += 1
                await message.edit(embed = embed2)
                await message.remove_reaction(reaction, user)
            elif str(reaction.emoji) == "▶️" and cur_page == 2:
                cur_page += 1
                await message.edit(embed = embed)
                await message.remove_reaction(reaction, user)

            elif str(reaction.emoji) == "◀️" and cur_page == 2:
                cur_page -= 1
                await message.edit(embed = poll_embed)
                await message.remove_reaction(reaction, user)
            elif str(reaction.emoji) == "◀️" and cur_page == 3:
                cur_page -= 1
                await message.edit(embed = embed2)
                await message.remove_reaction(reaction, user)

            else:
                await message.remove_reaction(reaction, user)
                # removes reactions if the user tries to go forward on the last page or
                # backwards on the first page
        except asyncio.TimeoutError:  # ending the loop if user doesn't react after x seconds
            await message.delete()
            break

@bot.command(name='spam')
@commands.has_any_role(885846416205967371, 886168003253776405)
async def spam(ctx, amount: int, *, message):
    for i in range(amount):  # Do the next thing amount times
        await ctx.send(message)  # Sends message where command was called

@bot.command(aliases = ['hspam'])
async def _hspam(ctx):
    embed_for_hspam = discord.Embed(title="$spam command help", description="to activate this command type:\n $spam (amount of messages) (the message)\n\nEXAMPLE:\n$spam 10 hello", colour=discord.Colour.gold())
    await ctx.send(embed= embed_for_hspam)
bot.run(TOKEN)
