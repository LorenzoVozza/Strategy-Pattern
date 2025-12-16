# Strategy-Pattern
Description 

import string
import random
from typing import List
from abc import ABC, abstractmethod


# helper function generates ticket IDs

def generate_id(length=8):
    return ''.join(random.choices(string.ascii_uppercase, k=length))



# represents a customer issue

class SupportTicket:

    def __init__(self, customer, issue):
        self.id = generate_id()     
        self.customer = customer    
        self.issue = issue          



# every strategy implements a ordering

class TicketOrderingStrategy(ABC):

    @abstractmethod
    def create_ordering(self, list: List[SupportTicket]) -> List[SupportTicket]:
        pass


# returns the list in the same order 

class FIFOOrderingStrategy(TicketOrderingStrategy):

    def create_ordering(self, list: List[SupportTicket]) -> List[SupportTicket]:
        return list.copy()  


# reverses the list making the new ticket is first

class FILOOrderingStrategy(TicketOrderingStrategy):

    def create_ordering(self, list: List[SupportTicket]) -> List[SupportTicket]:
        list_copy = list.copy()
        list_copy.reverse()
        return list_copy


# shuffles the ticket order 

class RandomOrderingStrategy(TicketOrderingStrategy):

    def create_ordering(self, list: List[SupportTicket]) -> List[SupportTicket]:
        list_copy = list.copy()
        random.shuffle(list_copy)
        return list_copy



# returns empty list so no tickets will are processed

class BlackHoleStrategy(TicketOrderingStrategy):

    def create_ordering(self, list: List[SupportTicket]) -> List[SupportTicket]:
        return []  



# holds tickets and processes them 

class CustomerSupport:

    def __init__(self, processing_strategy: TicketOrderingStrategy):
        self.tickets = []                 
        self.processing_strategy = processing_strategy  

# creates and stores new ticket   
    def create_ticket(self, customer, issue):
        self.tickets.append(SupportTicket(customer, issue))

# processes tickets using the strategy
    def process_tickets(self):
        
        ticket_list = self.processing_strategy.create_ordering(self.tickets)

# handles empty list case
        if len(ticket_list) == 0:
            print("There are no tickets to process. Well done!")
            return

# processes each ticket in order       
        for ticket in ticket_list:
            self.process_ticket(ticket)

    # prints ticket details 
    def process_ticket(self, ticket: SupportTicket):
        print("==================================")
        print(f"Processing ticket id: {ticket.id}")
        print(f"Customer: {ticket.customer}")
        print(f"Issue: {ticket.issue}")
        print("==================================")

# creates the app with a RANDOM order strategy

app = CustomerSupport(RandomOrderingStrategy())

# example tickets
app.create_ticket("Cat", "Why am I barking!")
app.create_ticket("Dog", "Why am I meowing!")
app.create_ticket("Cow", "MOOOOO!")
app.process_tickets()
