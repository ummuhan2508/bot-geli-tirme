import discord
from discord.ext import commands
import random
intents = discord.Intents.default()
intents.message_content = True
bot = commands.Bot(command_prefix='$', intents=intents)
hedef_sayilar = {}
@bot.event
async def on_ready():
    print(f'{bot.user} olarak giriş yaptık')
@bot.command()
async def tahmin(ctx, sayi: int = None):
    if sayi is None:
        hedef_sayilar[ctx.author.id] = random.randint(1, 20)
        await ctx.send("1 ile 20 arası bir sayı tuttum! Tahmin et: `$tahmin 5`")
        return
    
    if ctx.author.id not in hedef_sayilar:
        hedef_sayilar[ctx.author.id] = random.randint(1, 20)
    
    hedef = hedef_sayilar[ctx.author.id]
    
    if sayi == hedef:
        await ctx.send(f"🎉 Doğru! Sayı {hedef}'di!")
        del hedef_sayilar[ctx.author.id]
    elif sayi < hedef:
        await ctx.send("⬆️ Daha yüksek!")
    else:
        await ctx.send("⬇️ Daha düşük!")
bot.run("Token Buraya")
