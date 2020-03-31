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

if __name__=='__main__':
    father=Relation()
    mother=Relation()
    
    with open('relationships.json') as f:
        d=json.loads(f.read())

    for item in d['father']:
        facts(father,(list(item.keys())[0],list(item.values())[0]))

    for item in d['mother']:
        facts(mother,(list(item.keys())[0],list(item.values())[0]))

    a=var()
    
    # Suresh's children
    name='Suresh'
    output=run(0,a,father(name,a))
    print("\nList of " + name + "'s children:")
    for item in output:
        print(item)
