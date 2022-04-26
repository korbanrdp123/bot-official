# bot-official#include <a_samp>
#include <discord-connector>

new DCC_Channel:g_Discord_Chat;

public OnFilterScriptInit()
{
	print("\n===================================");
	print("|      DISCORD IRC SYSTEM V.1.0      |");
	print("|   BY CYBERH4WK LOADED SUCCESFULL   |");
	print("=====================================\n");
	g_Discord_Chat = DCC_FindChannelById("967338720946704438"); // Discord channel ID
    return 1;
}

forward DCC_OnMessageCreate(DCC_Message:message);

public DCC_OnMessageCreate(DCC_Message:message)
{
	new realMsg[100];
    DCC_GetMessageContent(message, realMsg, 100);
    new bool:IsBot;
    new DCC_Channel:channel;
 	DCC_GetMessageChannel(message, channel);
    new DCC_User:author;
	DCC_GetMessageAuthor(message, author);
    DCC_IsUserBot(author, IsBot);
    if(channel == g_Discord_Chat && !IsBot) //!IsBot will block BOT's message in game
    {
        new user_name[32 + 1], str[152];
       	DCC_GetUserName(author, user_name, 32);
        format(str,sizeof(str), "{8a6cd1}[Discord Announcement] {aa1bb5}%s: {ffffff}%s",user_name, realMsg);
        SendClientMessageToAll(-1, str);
    }

    return 1;
}

public OnPlayerText(playerid, text[])
{

    new name[MAX_PLAYER_NAME + 1];
    GetPlayerName(playerid, name, sizeof name);
    new msg[128]; 
    format(msg, sizeof(msg), "```%s: %s```", name, text);
    DCC_SendChannelMessage(g_Discord_Chat, msg);
    return 1;
}



public OnPlayerConnect(playerid)
{
   	new name[MAX_PLAYER_NAME + 1];
    GetPlayerName(playerid, name, sizeof name);

    if (_:g_Discord_Chat == 0)
    g_Discord_Chat = DCC_FindChannelById("769897680645390369"); // Discord channel ID

    new string[128];
    format(string, sizeof string, " **%s Telah masuk ke Kota Star Pride :)**", name);
    DCC_SendChannelMessage(g_Discord_Chat, string);
    return 1;
}

public OnPlayerDisconnect(playerid, reason)
{
    new name[MAX_PLAYER_NAME + 1];
    GetPlayerName(playerid, name, sizeof name);

    if (_:g_Discord_Chat == 0)
    g_Discord_Chat = DCC_FindChannelById("769897680645390369"); // Discord channel ID

    new string[128];
    format(string, sizeof string, " **%s Telah keluar dari Kota Star Pride :(**", name);
    DCC_SendChannelMessage(g_Discord_Chat, string);
    return 1;
}
