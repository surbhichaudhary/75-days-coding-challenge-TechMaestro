class LFUCache {
public:
	LFUCache(int capacity) : capacity(capacity) { }

	int get(int key) {
		if (capacity == 0) return -1;
		if (m.end() == m.find(key)) return -1;
		list<Item>::iterator item_it = m[key];
		refresh(item_it);
		return item_it->value;
	}

	void put(int key, int value) {
		if (capacity == 0) return;
		if (m.end() == m.find(key)) {
			if (m.size() == capacity) {
				// remove least frequently used
				int key_to_remove = l.begin()->item.back().key;
				l.begin()->item.pop_back();
				if (l.begin()->item.empty())
				    l.pop_front();
				m.erase(key_to_remove);
			}
			if (l.empty()) // cache is empty
			    l.push_front(Frequency(1)); 
			if (l.front().frequency > 1) // no keys of frequency 1
			    l.push_front(Frequency(1)); 
			l.begin()->item.push_front(Item(key, value, l.begin()));
			m[key] = l.begin()->item.begin();
		} else {
			list<Item>::iterator item_it = m[key];
			item_it->value = value;
			refresh(item_it);
		}
	}

private:
	struct Frequency;
	struct Item;
	void refresh(list<Item>::iterator item_it) {
		list<Frequency>::iterator frequency_it = item_it->frequency_it;
		int frequency = frequency_it->frequency;
		list<Frequency>::iterator plus_one_frequency_it = frequency_it;
		++plus_one_frequency_it;
		if (plus_one_frequency_it == l.end())
            l.push_back(Frequency(frequency + 1));
		else if (plus_one_frequency_it->frequency != frequency + 1) 
            l.insert(plus_one_frequency_it, Frequency(frequency + 1));
		plus_one_frequency_it = frequency_it;
		++plus_one_frequency_it;
		plus_one_frequency_it->item.splice(plus_one_frequency_it->item.begin(), frequency_it->item, item_it);
		if (frequency_it->item.empty())
		    l.erase(frequency_it);
		item_it->frequency_it = plus_one_frequency_it;
	}

private:
	struct Item {
		Item(int key, int value, list<Frequency>::iterator it) : key(key), value(value), frequency_it(it) {}
		int key;
		int value;
		list<Frequency>::iterator frequency_it;
	};
	struct Frequency {
		Frequency(int value) : frequency(value) {}
		int frequency;
		list<Item> item;  // list of <key, value, list<Frequency>::iterator>
	};
	list<Frequency> l;
	unordered_map<int, list<Item>::iterator> m;
	size_t capacity;
};
