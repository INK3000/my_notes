custom event for telethon


create custom_events/raw_magic.py

```python
import magic_filter as mf
from telethon.events.common import EventBuilder, EventCommon, name_inner_event


@name_inner_event
class RawMagic(EventBuilder):
    def __init__(self, magic=None):
        super().__init__()
        if isinstance(magic, mf.magic.MagicFilter):
            self.magic = magic
        else:
            raise Exception('magic is not a MagicFilter instance')

    async def resolve(self, client):
        self.resolved = True

    @classmethod
    def build(cls, update, others=None, self_id=None):
        return cls.Event(update.message)

    def filter(self, event):
        if self.magic:
            return self.magic.resolve(event)
        return super().filter(event)

    class Event(EventCommon):
        def __init__(self, message):
            self.__dict__['_init'] = False
            super().__init__(
                chat_peer=message.peer_id,
                msg_id=message.id,
                broadcast=bool(message.post),
            )
            self.message = message

        def _set_client(self, client):
            super()._set_client(client)
            m = self.message
            m._finish_init(client, self._entities, None)
            self.__dict__['_init'] = True  # No new attributes can be set

        def __getattr__(self, item):
            if item in self.__dict__:
                return self.__dict__[item]
            else:
                return getattr(self.message, item)

        def __setattr__(self, name, value):
            if not self.__dict__['_init'] or name in self.__dict__:
                self.__dict__[name] = value
            else:
                setattr(self.message, name, value)
```

in main.py:
```python
import asyncio
import logging
from telethon.tl import types
from rich import print
from .settings import settings
from magic_filter import F
from telethon import TelegramClient, events
from .custom_events import RawMagic

logging.basicConfig(
    format='[%(levelname) 5s/%(asctime)s] %(name)s: %(message)s',
    level=logging.INFO,
)


async def catch_animation(event: events.NewMessage.Event):
    client = event.client
    async with client.action(event.chat_id, 'typing'):   # pyright: ignore
        chat_id = event.chat_id
        sender_id = event.sender_id
        user = await event.get_sender()

        await event.respond(
            f'Catched! animation in {chat_id} by {user.first_name} with id: {sender_id}'
        )


async def catch_text(event: events.NewMessage.Event):
    client = event.client
    print(f'from catch_text event = {event.stringify()}')
    async with client.action(event.chat_id, 'typing'):   # pyright: ignore
        text = event.text
        chat_id = event.chat_id
        sender_id = event.sender_id
        user = await event.get_sender()

        await event.respond(
            f'Catched! {text} in {chat_id} by {user.first_name} with id: {sender_id}',
        )


async def catch_geo(event: events.NewMessage.Event):
    await event.respond('You send me location')


async def send_location(event: events.NewMessage.Event):
    chat_id = event.chat_id

    lat = 55.574806
    long = 26.441167

    lacation = types.InputGeoPoint(lat=lat, long=long)
    await event.client.send_file(chat_id, types.InputMediaGeoPoint(lacation))


async def notify_in_groups(event):
    action = ''
    user = await event.get_user()
    if event.user_added:
        action = 'user_added'
    if event.user_joined:
        action = 'user_joined'
    if event.user_left:
        action = 'user_left'
    if event.user_kicked:
        action = 'user_kicked'
    await event.delete()

    await event.respond(f'User {user.first_name} undergos {action}')


async def catch_raw(event):
    ...
    print(f'from catch_raw event = {event.stringify()}')
    client = event.client
    async with client.action(event.chat_id, 'typing'):   # pyright: ignore
        chat_id = event.chat_id
        sender_id = event.sender_id
        user = await event.get_sender()
        print(
            f'from catch_raw chat_id = {chat_id}, sender_id = {sender_id}, user = {user}'
        )
        await asyncio.sleep(5)

        await event.respond('catch_raw')

    # print('*' * 20)
    # print(event.stringify())
    # raise events.StopPropagation


def main():
    with TelegramClient(
        'bot/bot.session', settings.api_id, settings.api_hash
    ) as client:
        client.parse_mode = 'html'

        client.add_event_handler(catch_raw, RawMagic(F.is_private))

        client.add_event_handler(notify_in_groups, events.ChatAction)

        client.add_event_handler(
            catch_text, events.NewMessage(func=lambda e: e.text)
        )
        client.add_event_handler(
            catch_animation, events.NewMessage(func=lambda e: e.gif)
        )
        client.add_event_handler(
            catch_geo, events.NewMessage(func=lambda e: e.geo)
        )
        client.add_event_handler(
            send_location, events.NewMessage(pattern=r'^Пришли локацию$')
        )

        client.run_until_disconnected()


if __name__ == '__main__':
    main()
```


[[Python Libraries]]
