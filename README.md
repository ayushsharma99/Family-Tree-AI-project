import json
from logpy import Relation, facts, run, conde, var, eq
# To Check that if 'a' is the parent of 'b'
def parent(a, b):
    return conde([father(a, b)], [mother(a, b)])

# To Check that if 'a' is the grandparent of 'b'
def grandparent(a, b):
    temp = var()
    return conde((parent(a,temp),parent(temp,b)))

# To Check for sibling relationship between 'm' and 'n'  
def sibling(a, b):
    temp = var()
    return conde((parent(temp,a), parent(temp,b)))
    
# Check if m is n's uncle
def uncle(a,b):
    temp=var()
    return conde((father(temp,a),grandparent(temp,b)))
